# Day 7

# My answers

![Chef Answers](../images/day7/results1.png)

![My Answers](../images/day7/myanswers.png)
# The Story

![Shows AOC day 7 Image Head](../images/day7/day7header.png)

In the previous task, we learned that McSkidy was indeed a victim of a spearphishing campaign that also contained a suspicious-looking document Division_of_labour-Load_share_plan.doc. McSkidy accidentally opened the document, and it's still unknown what this document did in the background. McSkidy has called on the in-house expert Forensic McBlue to examine the malicious document and find the domains it redirects to. Malicious documents may contain a suspicious command to get executed when opened, an embedded malware as a dropper (malware installer component), or may have some C2 domains to connect to.

Learning Objectives

- What is CyberChef
- What are the capabilities of CyberChef
- How to leverage CyberChef to analyze a malicious document
- How to deobfuscate, filter and parse the data

# Lab Deployment

For today's task, you will need to deploy the machine attached to this task by pressing the green "Start Machine" button located at the top-right of this task. The machine should launch in a split-screen view. If it does not, you will need to press the blue "Show Split View" button near the top-right of this page.

CyberChef Overview

CyberChef is a web-based application - used to slice, dice, encode, decode, parse and analyze data or files. The CyberChef layout is explained below. An offline version of cyberChef is bookmarked in Firefox on the machine attached to this task.

CyberChef Interface

    Add the text or file in panel 1.
    Panel 2 contains various functions, also known as recipes that we use to encode, decode, parse, search or filter the data.
    Drag the functions or recipes from Panel 2 into Panel 3 to create a recipe.
    The output from the recipes is shown in panel 4.
    Click on bake to run the functions added in Panel 3 in order. We can select AutoBake to automatically run the recipes as they are added.

Using CyberChef for mal doc analysis

Let's utilize the functions, also known as recipes, from the left panel in CyberChef to analyze the malicious doc. Each step is explained below:

1) Add the File to CyberChef

Drag the invoice.doc file from the desktop to panel 1 as input, as shown below. Alternatively, the user can add the Division_of_labour-Load_share_plan.doc file by Open file as input icon in the top-right area of the CyberChef page.

Shows how to drag a file into CyberChef as input

2) Extract strings

Strings are ASCII and Unicode-printable sequences of characters within a file. We are interested in the strings embedded in the file that could lead us to suspicious domains. Use the strings function from the left panel to extract the strings by dragging it to panel 3 and selecting All printable chars as shown below:

Extract strings using strings function

If we examine the result, we can see some random strings of different lengths and some obfuscated strings. Narrow down the search to show the strings with a larger length. Keep increasing the minimum length until you remove all the noise and are only left with the meaningful string, as shown below:

Filter strings by the size using strings function

3) Remove Pattern

Attackers often add random characters to obfuscate the actual value. If we examine, we can find some repeated characters [ _ ]. As these characters are common in different places, we can use regex (regular expressions) within the Find / Replace function to find and remove these repeated characters.

To use regex, we will put characters within the square brackets [ ] and use backslash \ to escape characters. In this case, the final regex will be [\[\]\n_] where \n represents the Line feed, as shown below:

Use Regex in Find/Replace to filter data

It's evident from the result that we are dealing with a PowerShell script, and it is using base64 Encoded string to hide the actual code.

4) Drop Bytes

To get access to the base64 string, we need to remove the extra bytes from the top. Let's use the Drop bytes function and keep increasing the number until the top bytes are removed.

Use drop bytes to drop unwanted bytes
5) Decode base64

Now we are only left with the base64 text. We will use the From base64 function to decode this string, as shown below:

Use from Base64 to decode text from Base84 encoded value

6) Decode UTF-16

The base64 decoded result clearly indicates a PowerShell script which seems like an interesting finding. In general, the PowerShell scripts use the Unicode UTF-16LE encoding by default. We will be using the Decode text function to decode the result into UTF-16E, as shown below:

Decode to UTF-16LE using decode text function

7) Find and Remove Common Patterns

Forensic McBlue observes various repeated characters  ' ( ) + ' ` " within the output, which makes the result a bit messy. Let's use regex in the Find/Replace function again to remove these characters, as shown below. The final regex will be ['()+'"`].

Use regex within Find/Replace to filter data

8) Find and Replace
If we examine the output, we will find various domains and some weird letters ]b2H_ before each domain reference. A replace function is also found below that seems to replace this ]b2H_ with http.

Shows usage of Find/Replace function

Let's use the find / Replace function to replace ]b2H_ with http as shown below:

Use Replace/Replace function to replace chars

9) Extract URLs

The result clearly shows some domains, which is what we expected to find. We will use the Extract URLs function to extract the URLs from the result, as shown below:

Use Extract URLs function to extract URLs from the data

10) Split URLs with @

The result shows that each domain is followed by the @ character, which can be removed using the split function as shown below:

Use split function to split lines

11) Defang URL

Great - We have finally extracted the URLs from the malicious document; it looks like the document was indeed malicious and was downloading a malicious program from a suspicious domain.

Before passing these domains to the SOC team for deep malware analysis, it is recommended to defang them to avoid accidental clicks. Defanging the URLs makes them unclickable. We will use Defang URL to do the task, as shown below:

Use Defang to defang URLs to make them unclickable

Great work! 

It's time to share the URLs and the malicious document with the Malware Analysts.