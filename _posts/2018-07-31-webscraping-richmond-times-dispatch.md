---
layout: post
published: true
title: Webscraping - Richmond Times Dispatch
---
These are the steps I took in order to download the issues of "Richmond Times Dispatch" from the website of the Perseus Project:

1. I had a look at the issues on the website and consulted the materials you recommended (https://programminghistorian.org/en/lessons/automated-downloading-with-wget, https://programminghistorian.org/en/lessons/applied-archival-downloading-with-wget), and I also had a look at a manual which was recommended in one of the tutorials: https://www.gnu.org/software/wget/manual/wget.html#Very-Advanced-Usage

2. I had to identify that part of the link which would change from issue to issue so that I could write my script to create a loop in a txt file. I realized that the last four digits of the link were the variable, so the last four digits of the first issue were 0001, whereas the last four digits of the last issue were 1355, and these last four digits of every issue were in numerical order.

3. The problem was that I needed not only the first page of every issue, but all pages of all of these issues. I realized that I could not write a loop for every single page of every single issue to download everything, since the pages were only partly sorted in numerical orders. Sometimes additional characters were added, and these additional characters appeared after what seemed to me random pages in every single issue, so I had no idea how to write a Python script that would create a loop for all of these pages.

4. I tried downloading the whole first issue using the mirror command to see what happens. I got a number of documents in my folder, some of them were clearly the pages of the first issue in html format but without extension added to the documents. I tried a different command that should convert every html file that has officially no extension to an official html file as they are downloaded, but the command didn't work for some reason. At this point, I thought the mirror command could solve my problem, as I could use the main link of every single issue to download everything, even though I had my doubts because this way I'd consume too much time downloading really everything, even those documents that I didn't really need. I tried a different code that should download only html files, but as the files were officially not html files, for they did not have the extension, I couldn't download a thing. Seeing that I had no better idea, I decided to write a script to make a loop at least with the main links of every issue.

5. As the range had to be between 0001 and 1355 and it had many leading zeros that I could not leave in the string, I wrote a Python script for leading zeros, but the script apparently didn't work, the txt file was empty. If I tried using a different link in the script, it worked very well, but not with the link I needed. I realized that there had to be some special characters in the link that my script didn't really like, so I got rid of the part that was suspicious to me (=Perseus%3atext%) and I replaced it with a simple combination of letters that did not occur in my script otherwise: xyz. I run the script which worked perfectly this time, then I opened the txt file and used the search and replace function to change the xyz combination to =Perseus%3atext%, and I was ready within a second.

This is what my script looked like:

#Richmond-times.py

urls = '';
f=open('richmond-times.txt','w')

for w in range(1,10):
    urls += 'http://www.perseus.tufts.edu/hopper/text?docxyz3a2006.05.000%d\n' % (w)

for x in range(10,100):
    urls += 'http://www.perseus.tufts.edu/hopper/text?docxyz3a2006.05.00%d\n' % (x)

for y in range(100,1000):
    urls += 'http://www.perseus.tufts.edu/hopper/text?docxyz3a2006.05.0%d\n' % (y)

for z in range(1000,1356):
    urls += 'http://www.perseus.tufts.edu/hopper/text?docxyz3a2006.05.%d\n' % (z)

f.write(urls)
f.close

6. The problem was, however, that the wget command that should download contents using certain links in a txt file didn't work for some reason. I could not mirror the content of these links, I could not use other methods to download what I wanted, so I had no idea how I should proceed. I had a look at the website again, and I saw that it was possible to download the pages in xml format. I had already seen that this was possible, but again, the problem was that the links were only partly in numerical order. I soon realized, however, that there was a button to download what I soon realized to be the complete issue in xml format. It would make my task easier if I only had to download every single issue instead of downloading every single page of every single issue.

7. I copied the download link of several issues and I realized that they were sorted in numerical order! I only had to write a script to have a loop, and I could download every single issue! As the download link was very similar to the links I used in my script earlier, I used the same method. I just had to change my earlier script a little bit where the link had differencies, I run the script, I used the search and replace function to replace the problematic parts and I had a txt file with the download links of every single issue.

This is the script I used to get the download links: 
#Richmond-times.py

urls = '';
f=open('richmond-times.txt','w')

for w in range(1,10):
    urls += 'http://www.perseus.tufts.edu/hopper/dltext?docxyz3A2006.05.000%d\n' % (w)

for x in range(10,100):
    urls += 'http://www.perseus.tufts.edu/hopper/dltext?docxyz3A2006.05.00%d\n' % (x)

for y in range(100,1000):
    urls += 'http://www.perseus.tufts.edu/hopper/dltext?docxyz3A2006.05.0%d\n' % (y)

for z in range(1000,1356):
    urls += 'http://www.perseus.tufts.edu/hopper/dltext?docxyz3A2006.05.%d\n' % (z)

f.write(urls)
f.close

8. I used wget to download every single issue between 1860 and 1865. This is what the result looks like:
![](/img/richmond-times.PNG)

