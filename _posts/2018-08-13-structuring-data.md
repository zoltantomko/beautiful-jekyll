---
layout: post
published: true
title: Structuring Data
---
This exercise was not very difficult, but kind of tricky. It was easier to solve because we could use the code from the last assignment, we just had to modify it a little bit to make sure that we get one tsv file in the end. It was helpful that we had a discussion about how to solve this assignment with the others from our group, there were several ideas that could equally work. My code was still not perfect for a long time, even though the brainstorming definitely helped, but eventually I realized that my code was not working because I was sloppy and forgot to add one small line. After I realized what the problem was, my code worked perfectly.

import re, os

source = "C:\\Users\\Zoli\\Documents\\XML\\"
target = "C:\\Users\\Zoli\\Documents\\XML\\Structuring\\"

list = []

lof = os.listdir(source)
counter = 0 # general counter to keep track of the progress

for f in lof:
    if f.startswith("dltext"): # fileName test        
        with open(source + f, "r", encoding="utf8") as fl:
            text = fl.read()

            # try to find the date
            date = re.search(r'<date value="([\d-]+)"', text).group(1)

            # splitting the issue into articles/items
            split = re.split("<div3 ", text)

            c = 0 # item counter
            for s in split[1:]:
                c += 1
                s = "<div3 " + s # a step to restore the integrity of items
                #input(s)

                # try to find a unitType
                try:
                    unitType = re.search(r'type="([^\"]+)"', s).group(1)
                except:
                    unitType = "noType"
                    print(s)

                # try to find a header
                try:
                    header = re.search(r'<head>(.*)</head>', s).group(1)
                    header = re.sub("<[^<]+>", "", header)
                except:
                    header = "NO HEADER"

                text = re.sub("<[^<]+>", "", s)
                text = re.sub(" +\n|\n +", "\n", text)
                text = re.sub("\n+", ";;; ", text)


                itemID = date+"_"+unitType+"_"+str(c)
                dateVar   = "#DATE: " + date
                unitType = "#TYPE: " + unitType
                header = "#HEADER: " + header
                text = "#TEXT: " + text

                # creating a text variable
                var = "\t".join([itemID,dateVar,unitType,header,text])+ "\n"
                #input(var)
                
		#appending variable to list
                list.append(var)

                results = "\n".join(list)

    # saving
    with open(target+ "/" + "result" +".tsv", "w", encoding="utf8") as f9:
                f9.write(results)
                            
