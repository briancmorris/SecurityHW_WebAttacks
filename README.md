# README
## Author

name: Brian C. Morris

email: briancmorris@live.com

## Level 1
### Vulnerability:
The author of the webpage has hidden the flag inside of an HTML comment.
### Description:
I inspected the HTML elements by opening Chrome's webtools (F12) and clicked the elements tab. The flag could be found embedded in an HTML comment contained within the body tag. The flag is: flag{inplainsight}

## Level 2
### Vulnerability:
The author of the webpage has hidden the flag behind a password, but has not hidden the script that generates the flag.
### Description:
I inspected the HTML elements by opening Chrome's webtools (F12) and clicked the elements tab. By examining the body tag, and then the script tag, I could see the JavaScript that generates the flag's value. At the end of the script, I observed that flag is set to the value of cflag. This means that if I can determine the generated cflag, I know what the flag is. By examining the JavaScript, I saw that the string obf is split into an array of substrings by using the delimiter "_". The script then calls the JavaScript function: String.fromCharCode() for each value in the array. By looking at the documentation of String.fromCharCode(), I learned that the function converts the given string to an ASCII character. By converting each of the ASCII codes stored in obf, I got the result: flag{j4v45cr1p7_15_7h3_fu7ur3}

The documentation for the function String.fromCharCode() can be found here: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/fromCharCode

## Level 3
### Vulnerability:
The author of the webpage allows indicates if the password is incorrect in real time. Allowing a user to easily brute-force the password.
### Description:
I noticed that when I input most characters into the password field that the page immediately loaded an error message indicating that I input the wrong password. When I cleared the password field and input the character 'f', the error message disappeared. This indicated that my input was checked against the valid password on every keystroke.

I inspected the HTML elements by opening Chrome's webtools (F12) and clicked the elements tab. I examined the body tag and started typing input to the password field. I noticed that the div tag labeled with the class "errors" only populated when an incorrect character was input. This confirmed that my input was checked in real time.

Knowing that all of the flags, so far, started with flag{ I input this string and no error message appeared. I continued to manually brute force the password until I reached the closing brace of the flag. This resulted in the flag: flag{bytebybytegoesyoursecurity}

Clicking the submit button resulted in a success message, indicating that I had found the flag.

## Level 4
### Vulnerability:
The author of the webpage has the flag revealed in the header of the server response.
### Description:
I inspected the HTML elements by opening Chrome's webtools (F12) and clicked the elements tab. While inspecting the webpage I did not notice anything out of the ordinary, so I began to experiment with burp to see what I could find.

After burp was started, I turned intercept off under the proxy tab so that the webpage could load completely. I then navigated to the homework page for problem 4. On the target tab, I expanded the drop-down menu for https://hw1.kapravelos.com, expanded the hw1 folder, then clicked the webpage labeled 4. In the center window, I clicked the response tab for webpage 4. The flag labeled X-Flag was clearly visible. The listed flag was: flag{cust0mHe@dersisthefutur3}

## Level 5
### Vulnerability:
The author of the webpage allows the user to overwrite their cookies to gain admin privileges.
### Description:
I inspected the HTML elements by opening Chrome's webtools (F12) and clicked the elements tab. While inspecting the webpage I did not notice anything out of the ordinary, so I began to experiment with burp to see what I could find.

After burp was started, I left intercept on under the proxy tab so that the webpage could be in steps. I then navigated to the homework page for problem 5. I continued to press the "forward" button until the webpage loaded completely, noting how the cookie changed as the 
page loaded. Initially, nothing was stored except for the session value. I then refreshed the page and continued to press the "forward" button until the new cookie was shown. I then noticed that there was a new value stored in the cookie called: auth=eyJhZG1pbiIgOiBmYWxzZX0=. I immediately thought that this was an encoded authentication token and researched common encoding methods for cookies. I found that base 64 was commonly used, and attempted to decode the auth token using a website which yielded the result: {"admin" : false}

I then changed the resulting string to {"admin" : true} and re-encoded the auth token to base 64 using the same website. I then cut and paste the newly encoded auth value (auth=eyJhZG1pbiIgOiB0cnVlfQ==) into burp where the auth token was stored. Pressing the "forward" button again revealed the token: flag{c00ki3s3curit7sisam0nst3r}

The website I used to encode and decode the tokens in base 64 was: https://www.base64decode.org/

## Level 6
### Vulnerability:
The author of the webpage checks for a specific web browser's User-Agent which can be overwritten by a user to gain access to the flag.
### Description:
I inspected the HTML elements by opening Chrome's webtools (F12) and clicked the elements tab. While inspecting the webpage I did not notice anything out of the ordinary, so I began to experiment with burp to see what I could find.

After burp was started, I left intercept on under the proxy tab so that the webpage could be in steps. I then navigated to the homework page for problem 6. I continued to press the "forward" button until the webpage loaded completely. I inspected the HTTP header and cookies until I saw the header value: "User-Agent". Since the webpage specified that it was only compatible with Mosaic running on Windows, I researched some common User-Agents that were used with Mosaic. In Burp, I changed my User-Agent to: "Mosaic/0.2 (Windows 95)". I then pressed the forward button and the flag was revealed. The flag was: flag{0ldsch00lbr0wser}

The website I used to research Mosaic user-agents was: https://developers.whatismybrowser.com/useragents/explore/software_name/mosaic/

## Level 7
### Vulnerability:
The author of the webpage does not check what type of file is uploaded and therefore php code can be executed to reveal the flag.
### Description:
I first tried uploading a picture to see how it changed the contents of the HTML page. I inspected the HTML elements by opening Chrome's webtools (F12) and clicked the elements tab. I saw that the name of the picture I uploaded could be found within the body->center->div->img tag. The image source was specified to be located in a subfolder called uploads. This indicated two things to me. First, the author doesn't check the type of input file, and blindly displays the file after uploading. If a php file is input, the php code will execute natively, since it is being injected directly into the webpage.

Now knowing where my uploaded file is stored, I uploaded a php file (named: haxor.php) to echo the contents of flag.txt.

The following code was uploaded:

<?php
echo file_get_contents("../flag.txt");
?>

Note: the image source is stored in a subfolder, so I needed to add "../" to the php code to ensure that the contents of flag.txt would be echoed.

By navigating to the location of my stored file (http://hw1.kapravelos.com:8087/uploads/haxor.php), I was able to retrieve the flag. The flag was: flag{3x3cut3afterupl0ad}.

## Level 8
### Vulnerability:
The author of the webpage allows the user to overwrite variables stored on the webpage. In particular, the variable $filename is what causes the vulnerability. By overwriting this variable, I am able to bypass the requirements to access the flag.
### Description:
First, I input a guess '1' and clicked the guess button. I noticed that the URL changed to: http://hw1.kapravelos.com:8088/index.php?attempt=1, noting that it appeared to assign a value to a variable named 'attempt'.

I inspected the HTML elements by opening Chrome's webtools (F12) and clicked the elements tab. By expanding the body tag, I observed that there was an unused anchor tag, a, that navigates to a new webpage. Notably, there is no text within the anchor tag, so the link will never appear on the webpage. By adding text, I can then click the link (or manually type in the link) to take me to a new webpage: http://hw1.kapravelos.com:8088/?hl

Here I saw that the PHP code checks my guesses (stored in $attempt) against the contents of the file: 'secret-combination.txt' (stored in $combination). Because of how the author implemented the webpage, I can overwrite the variable $filename by specifying a value within the URL to a filename that does not exist on the system. In doing so, the value of $combination is empty, and I can then insert an empty value for attempt.

By changing the URL to: http://hw1.kapravelos.com:8088/index.php?filename=blah&attempt= I overwrite $filename to be 'blah' and attempt to be an empty string. Navigating to the above URL results in a page that reveals the flag: flag{extractallthevariables!}

## Level 9
### Vulnerability:
The author of the webpage allows the user to overwrite URL parameters that execute a PHP function. By editing the cookie value of "challenge", a user can access the flag by injecting PHP code.
### Description:
The first thing I did upon reaching the webpage was open the source code to inspect. Observing the php code below the HTML shows us a switch statement that changes on the page URL that is input. The default page, introduction page, and privacy page indicate nothing of interest. Notably, the contactus page randomly generates a value and stores it in the variable $rand. It then sets the cookie value 'challenge'. The captcha page then echoes the value of challenge.

By navigating to contactus (http://hw1.kapravelos.com:8089/?page=contactus) and then captcha (http://hw1.kapravelos.com:8089/?page=captcha), I can see the value of challenge.

Finally, when examining the captcha-verify page in the PHP code, I can see the definition for a function called verifyFromMath. The function is called if the request variable 'answer' and 'method' are set to some value. The if statement allows me to change the values of answer and method to execute verifyFromMath. verifyFromMath directly inputs the value of challenge to the webpage. By changing the value of challenge to a PHP code fragment, I can execute the code on the webpage.

After launching burp, turn off intercept and navigate to the contactus page (http://hw1.kapravelos.com:8089/?page=contactus). This generates the value of challenge. Now, turn intercept on and attempt to load captcha-verify (http://hw1.kapravelos.com:8089/?page=captcha-verify). Click the params tag of the prompt that appears. Note, the URL parameters answer and method are missing. I added these two parameters by clicking the "add" button, ensuring that it remained a URL variable, and entering their names. Double clicking the value field allowed me to change their values to a for answer and b for method. I then changed the value of challenge to: print_r(file_get_contents("flag.txt")). After clicking the forward button, the webpage is reloaded and displays the flag: flag{inj3ctingPHPis3vil}.

## Level 10
### Vulnerability:
The author of the webpage allows a user to append the contents of the flag.txt file to their PHP session file by using a script.
### Description:
The first thing I did upon opening up the problem was inspect the source code of the website. Like problem 9, the contactus webpage randomly generates a number, this time it is stored in a PHP session file. By navigating to the captcha page using the URL: http://hw1.kapravelos.com:8090/?page=captcha I could see the randomly generated value.

When inspecting the captcha-verify case in the switch statement, I noticed that the request looks for 2 URL parameters. First, it checks that the value of 'answer' is set to something. Then it checks that the value of 'method' is set to something. Finally, it checks if the function provided by the parameter 'method' is enabled in the configurations. If so, it calls the function from 'method', passing it the value of the random session file, and the value of 'answer'.

I needed functions that accept 2 arguments (filename, variable), and was enabled by the php configurations.

I used the following functions: file_put_contents, chmod, highlight_file, and popen.

First, I wrote a shell script to cat the contents of the flag.txt file, and then append it to my randomly generated session file.

This is the script that was used:

#!/bin/sh
cat /var/www/html/flag.txt>> "./605521";

Note: The document root is listed as: /var/www/html on the PHP configuration page.

Using this website: https://www.urlencoder.org/ I encoded my script to standard URL encoding schemes. Note: The website that generates the URL encoding inserts %0D%0A as the newline character, I removed the %0D to ensure it was compatible with Linux systems.

This provided me with the URL code: %23%21%2Fbin%2Fsh%0Acat%20%2Fvar%2Fwww%2Fhtml%2Fflag.txt%3E%3E%20%22.%2F605521%22%3B

By then navigating to the captcha-verify page and inserting file_put_contents for 'method' and the above code for 'answer', I was able to append the contents of the flag to my session file. The URL used was:

http://hw1.kapravelos.com:8090/?page=captcha-verify&method=file_put_contents&answer=%23%21%2Fbin%2Fsh%0Acat%20%2Fvar%2Fwww%2Fhtml%2Fflag.txt%3E%3E%20%22.%2F605521%22%3B

Now that I have the contents of the file appended to my session file, I need give myself permissions to execute the script using the chmod function. I set the value of 'method' to chmod and the value of 'answer' to 511 to give myself execute permissions. I used the following URL:

http://hw1.kapravelos.com:8090/?page=captcha-verify&method=chmod&answer=511

Once I gave myself permission to execute my script, I executed it with popen in mode 'r' to allow the contents of the flag file to be appended to my script. I set 'method' to popen and 'answer' to r using the following URL:

http://hw1.kapravelos.com:8090/?page=captcha-verify&method=popen&answer=r

Finally, I printed the contents of my script using the highlight_file function, showing the contents of my script on the webpage using the following URL:

http://hw1.kapravelos.com:8090/?page=captcha-verify&method=highlight_file&answer=

The following text containing the flag was displayed:

#!/bin/sh
cat /var/www/html/flag.txt>> "./605521";flag{inj3ctingPHPinS3SSI0NSis3vil}
