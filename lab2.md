# Lab Report Week 1
By Ekin Celik
## Part 1
Code:
```
import java.io.IOException;
import java.net.URI;

class Handler implements URLHandler 
{

    String myString = "";
    int n = 1;

    public String handleRequest(URI url) 
    {
     if (url.getPath().contains("/add-message")) 
     {
            String[] parameters = url.getQuery().split("=");
            if (parameters[0].equals("s")) 
            {
                myString+="\n";
                myString+=String.format("%d. ", n++);
                myString+=parameters[1];
                return myString;
            }
            else
            {
                return "404 Not Found!";
            }

     }
     else
     {
        return myString;
     }
    }
}

class StringServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```
Screenshots:
