# Day 6


# My Notes

# The Story


Elf McBlue found an email activity while analysing the log files. It looks like everything started with an email...

# Learning Objectives

- Learn what email analysis is and why it still matters.
- Learn the email header sections.
- Learn the essential questions to ask in email analysis.
- Learn how to use email header sections to evaluate an email.
- Learn to use additional tools to discover email attachments and conduct further analysis.
- Help the Elf team investigate the suspicious email received.

## What is Email Analysis?

Email analysis is the process of extracting the email header information to expose the email file details. The email header contains the technical details of the email like sender, recipient, path, return address and attachments. Usually, these details are enough to determine if there is something suspicious/abnormal in the email and decide on further actions on the email, like filtering/quarantining or delivering. This process can be done manually and with the help of tools.

There are two main concerns in email analysis.

- __Security issues__: Identifying suspicious/abnormal/malicious patterns in emails.
- __Performance issues__: Identifying delivery and delay issues in emails.

In this task, we will focus on security concerns on emails, a.k.a. phishing. Before focusing on the hands-on email analysis, you will need to be familiar with the terms "social engineering" and "phishing".

- __Social engineering__: Social engineering is the psychological manipulation of people into performing or divulging information by exploiting weaknesses in human nature. These "weaknesses" can be curiosity, jealousy, greed, kindness, and willingness to help someone.
- __Phishing__: Phishing is a sub-section of social engineering delivered through email to trick someone into either revealing personal information and credentials or executing malicious code on their computer.

Phishing emails will usually appear to come from a trusted source, whether that's a person or a business. They include content that tries to tempt or trick people into downloading software, opening attachments, or following links to a bogus website. You can find more information on phishing by completing the phishing module.

## Does the Email Analysis Still Matter?

Yes! Various academic research and technical reports highlight that phishing attacks are still extremely common, effective and difficult to detect. It is also part of penetration testing and red teaming implementations (paid security assessments that examine organisational cybersecurity). Therefore, email analysis competency is still an important skill to have. Today, various tools and technologies ease and speed up email analysis. Still, a skilled analyst should know how to conduct a manual analysis when there is no budget for automated solutions. It is also a good skill for individuals and non-security/IT people!

__Important Note__: In-depth analysis requires an isolated environment to work. It is only suggested to download and upload the received emails and attachments if you are in the authorised team and have an isolated environment. Suppose you are outside the corresponding team or a regular user. In that case, you can evaluate the email header using the raw format and conduct the essential checks like the sender, recipient, spam score and server information. Remember that you have to inform the corresponding team afterwards.

## How to Analyse Emails?

Before learning how to conduct an email analysis, you need to know the structure of an email header. Let's quickly review the email header structure.
Field	Details
From	

![Email header](../images/day6/emailtable.png)

## Important Email Header Fields for Quick Analysis

Analysing multiple header fields can be confusing at first glance, but starting from the key points will make the analysis process slightly easier. A simple process of email analysis is shown below.

![tablesemail2](../images/day6/table2.png)

You'll also need an email header parser tool or configure a text editor to highlight and spot the email header's details easily. The difference between the raw and parsed views of the email header is shown below.

Note: The below example is demonstrated with the tool "Sublime Text". The tool is configured and ready for task usage in the given VM. 

![raw vs highlighed header](../images/day6/1.png)

You can use Sublime Text to view email files without opening and executing any of the linked attachments/commands. You can view the email file in the text editor using two approaches.

- Right-click on the sample and open it with Sublime Text.
- Open Sublime Text and drag & drop the sample into the text editor.

![Open email file in a text editor](../images/day6/2.png)

If your file has a ".eml" or ".msg" extension, the sublime text will automatically detect the structure and highlight the header fields for ease of readability. Note that if you are using a ".txt" or any other extension, you will need manually select the highlighting format by using the button located at the lower right corner.

![Change highlight syntax](../images/day6/3.png)

Text editors are helpful in analysis, but there are some tools that can help you to view the email details in a clearer format. In this task, we will use the "emlAnalyzer" tool to view the body of the email and analyse the attachments. The emlAnalyzer is a tool designed to parse email headers for a better view and analysis process. The tool is ready to use in the given VM. The tool can show the headers, body, embedded URLs, plaintext and HTML data, and attachments. The sample usage query is explained below.

![emailalalyzer](../images/day6/table3.png)

Sample usage is shown below. Now use the given sample and execute the given command.

![terminal](../images/day6/terminal2.png)


At this point, you should have completed the following checks.

- Sender and recipient controls
- Return path control
- Email server control
- Message-ID control
- Spam value control 
- Attachment control (Does the email contains any attachment?)

Additionally, you can use some Open Source Intelligence (OSINT) tools to check email reputation and enrich the findings. Visit the given site below and do a reputation check on the sender address and the address found in the return path.


    Tool: https://emailrep.io/

![Email reputation check](../images/day6/4.png)

Here, if you find any suspicious URLs and IP addresses, consider using some OSINT tools for further investigation. While we will focus on using Virustotal and InQuest, having similar and alternative services in the analyst toolbox is worthwhile and advantageous.

![table](../images/day6/table5.png)

After completing the mentioned initial checks, you can continue with body and attachment analysis. Now, let's focus on analysing the email body and attachments. The sample doesn't have URLs, only an attachment. You need to compute the value of the file to conduct file-based reputation checks and further your analysis. As shown below, you can use the sha256sum tool/utility to calculate the file's hash value. 


Note: Remember to navigate to the file's location before attempting to calculate the file's hash value.


![emlAnalyzer Usage](../images/day6/analyzer1.png)


![VirusTotal](../images/day6/5.png)

Once you get the sum of the file, you can go for further analysis using the VirusTotal.


    Tool: https://www.virustotal.com/gui/home/upload

Now, visit the tool website and use the SEARCH option to conduct hash-based file reputation analysis. After receiving the results, you will have multiple sections to discover more about the hash and associated file. Sections are shown below.

Virustotal sections

- Search the hash value
- Click on the BEHAVIOR tab.
- Analyse the details.

After that, continue on reputation check on InQuest to enrich the gathered data.

- Tool: https://labs.inquest.net/

Now visit the tool website and use the INDICATOR LOOKUP option to conduct hash-based analysis.

- Search the hash value
- Click on the SHA256 hash value highlighted with yellow to view the detailed report.
- Analyse the file details.


![InQuest](../images/day6/6.png)


After finishing the shown steps, you are finished initial email analysis. The next steps are creating a report of findings and informing the team members/manager in the appropriate format.


Now is the time to put what we've learned into practice. Click on the Start Machine button at the top of the task to launch the Virtual Machine. The machine will start in a split-screen view. In case the VM is not visible, use the blue Show Split View button at the top-right of the page. Now, back to elf McSkidy analysing the suspicious email that might have helped the Bandit Yeti infiltrate Santa's network.


IMPORTANT NOTES:

- Given email sample contains a malicious attachment.
- Never directly interact with unknown email attachments outside of an isolated environment.

