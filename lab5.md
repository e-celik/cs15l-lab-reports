# Lab Report 5: Putting It All Together
By Ekin Celik
## Part 1: The Post

By Anonymous:

Hello, I am having trouble adding functionality to NumberServer. It already has increment and add funcionality, 
and I am now trying to add history and restore history funcitonality. For restore history, the problem arises 
when I attempt to restore the history relatively, as in, from a number of steps before the current state. 
The restore history funcitonality works as expected when I simply give it the number of a state to restore to.  
  
I have been looking for a long time, and I can't find what's wrong. It appears that most of the time, the restore 
simply returns 0, but sometimes, it has unexpected behavior and returns a seemingly random index in the history.
Sometimes, I even recieve an array out of bounds exception. 

Here is an example of when it returns a random number:  

![image](https://github.com/e-celik/cs15l-lab-reports/assets/145171936/3ee691bb-93c2-42b7-8620-8583f48ed19e)
![image](https://github.com/e-celik/cs15l-lab-reports/assets/145171936/fc5c7ad4-5c61-48ba-ab4a-1761c90b5bc5)
![image](https://github.com/e-celik/cs15l-lab-reports/assets/145171936/577ab1b0-26e1-4d8a-8624-66eb232ff18b)  
As you can see, It returns 2 instead of 3  


Here is an example of when it throws an exception:

![image](https://github.com/e-celik/cs15l-lab-reports/assets/145171936/894b295a-1dce-4994-ac2f-513d0e01043f)
![image](https://github.com/e-celik/cs15l-lab-reports/assets/145171936/34df027e-827e-4412-8111-1b2d3733261b)


## Part 2: TA Response  

By TA:  

Hello student. It's good that you could narrow your problem down to the case where you handle /restore?at. There are some issues with
some search engines' autocomplete feautres that can cause some issues. To avoid this, type out each url extension itstead of using the autocomplete feature. 
Other than that, consider what variables your implementation is using. You likely store the history in an array, and maintain the current number in a variable.
How are you keeping track of your array? Perhaps consider putting some print statements to help debug. These prints will show up in the terminal, not the server page.

## Part 3: Debugging
By Anonymous:  

![image](https://github.com/e-celik/cs15l-lab-reports/assets/145171936/36d4e81b-36a8-467f-8263-c907ff8113be)
![image](https://github.com/e-celik/cs15l-lab-reports/assets/145171936/a941ae39-1f1d-4202-8d73-e35767779e47)  

I noticed that I sometimes recieved "atback" in the terminal when I should have only expected "back."
This happened when chrome autofilled "at" and I allowed it to, so that I could change it to "back" before pressing enter.
When I typed the url out carefully, without autocompleting, this problem stopped happening. However, I still had a problem with the back functionality.
I am using an integer counter to keep track of how many elements are in my array. After considering where I used each of my variables in the implementation, 
I found the error. I had replaced numctr with num in the statement to retrieve the historical element with respect to the current element. This meant that instead of going n 
steps back from the position element A, I was going n steps back from the number stored in element A. This explains how I could get out of bounds, random numbers, and 0. If element A was large, 
then the value at the index I was retrieving would have been empty, hence 0. If the value at the index was small, then I would subtract too large a number from it, and attempt to get an element 
not in the array. Everything in between explains how I would get random numbers already in the array.

## Part 4: The Program

-~
  -Wavelet
    -Server.java
    -NumberServer.java
    -start.sh

start.sh:
```
set -e
javac Server.java NumberServer.java
java NumberServer $1
```
  
NumberServer.java
```
import java.io.IOException;
import java.net.URI;

class Handler implements URLHandler {
    // The one bit of state on the server: a number that will be manipulated by
    // various requests.
    int num = 0;
    int[] oldnums = new int[100];
    //ArrayList<Integer> oldnums = new ArrayList<Integer>(); 
    int numctr = 0;

    public String handleRequest(URI url) 
    {
        if (url.getPath().equals("/")) 
        {
            return String.format("My Number: %d", num);
        } 
        else if (url.getPath().equals("/increment")) 
        {
            oldnums[numctr] = num;
            num += 1;
            numctr++;
            return String.format("Number incremented!");
        } 
        else if (url.getPath().equals("/history")) 
        {
            String toReturn = "";
            int i;
            for (i = 0; i < numctr; i++)
            {
                toReturn += String.format("%d. %d\n", i, oldnums[i]);
            }
            toReturn += String.format("My Number: %d\n", num);
            return toReturn;
        } 
        else if (url.getPath().contains("/add")) 
        {
                String[] parameters = url.getQuery().split("=");
                if (parameters[0].equals("count")) 
                {
                    oldnums[numctr] = num;
                    num += Integer.parseInt(parameters[1]);
                    numctr++;
                    return String.format("Number increased by %s! It's now %d", parameters[1], num);
                }
            return "404 Not Found!";
        }
        else if (url.getPath().contains("/restore")) 
        {
                String[] parameters = url.getQuery().split("=");
                if (parameters[0].equals("at")) 
                {
                    System.out.print("at");
                    oldnums[numctr] = num;
                    num = oldnums[Integer.parseInt(parameters[1])];
                    numctr++;
                    return "History Restored";
                }
                else if (parameters[0].equals("back")) 
                {
                    System.out.print("back");
                    oldnums[numctr] = num;
                    num = oldnums[num - (Integer.parseInt(parameters[1]) + 1)];  //ERROR HERE
                    numctr++;
                    return "History Restored";
                }
                return "404 Not Found!";
        }
        else 
        {
            return "404 Not Found!";
        }
    }

class NumberServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
}
```
  
Server.java: 
```
// A simple web server using Java's built-in HttpServer

// Examples from https://dzone.com/articles/simple-http-server-in-java were useful references

import java.io.IOException;
import java.io.OutputStream;
import java.net.InetSocketAddress;
import java.net.URI;

import com.sun.net.httpserver.HttpExchange;
import com.sun.net.httpserver.HttpHandler;
import com.sun.net.httpserver.HttpServer;

interface URLHandler {
    String handleRequest(URI url);
}

class ServerHttpHandler implements HttpHandler {
    URLHandler handler;
    ServerHttpHandler(URLHandler handler) {
      this.handler = handler;
    }
    public void handle(final HttpExchange exchange) throws IOException {
        // form return body after being handled by program
        try {
            String ret = handler.handleRequest(exchange.getRequestURI());
            // form the return string and write it on the browser
            exchange.sendResponseHeaders(200, ret.getBytes().length);
            OutputStream os = exchange.getResponseBody();
            os.write(ret.getBytes());
            os.close();
        } catch(Exception e) {
            String response = e.toString();
            exchange.sendResponseHeaders(500, response.getBytes().length);
            OutputStream os = exchange.getResponseBody();
            os.write(response.getBytes());
            os.close();
        }
    }
}

public class Server {
    public static void start(int port, URLHandler handler) throws IOException {
        HttpServer server = HttpServer.create(new InetSocketAddress(port), 0);

        //create request entrypoint
        server.createContext("/", new ServerHttpHandler(handler));

        //start the server
        server.start();
        System.out.println("Server Started!");
    }
}
```
  
I did not run any commands to find the bug, other than starting the server with <bash start.sh ####>. 
However, I did lots of testing on the server, with lots and lots of url's, which are unnecessary to write here.
Here are a few examples of the url extensions I mixed and matched:
```
/
/history
/add?count=9
/increment
/restore?back=2
/restore?at=3
```
I used many combinations and permutations of the above.

Finally, in order to fix my error, I simply had to find the implementation of my restore?back command, 
and replace a "num" with "numctr." I have marked the location in NumberServer.java above with an inline comment showing where the fix should be applied


# Section 2: Reflection

One of the more useful and rewarding things I learned in the second half was how to use the command <echo $?> to help debug programs from the command line, 
immeadiately after they ran. I found myself using this command not only in this class, but also my other CS classes as well. It's useful because its quick to use, a
nd it gives you insight on the success of your program, even if you didn't include any output.
