---
layout: post
title: "R - running R scripts from batch files on Windows "
date: 2017-07-25
---

At [work](http://www.momondo.com) I have a couple of R-scripts set up to run automatically at various scheduled times (via the [Task Scheduler](http://www.thewindowsclub.com/how-to-schedule-batch-file-run-automatically-windows-7)). My work computer runs Windows 8, and having been a Mac-only for the last decade, I can not claim to be fluent in the Windows Command Prompt terminology. Therefor I wanted to share a couple of different ways to set up your .bat-files, in case you (like myself) can't be bothered with the documentation.

So, basically I have 3 different script running, depending on the needs of the script.

## Outputting R-log to text file (.Rout) while specifying R-version

I have built several scripts under several version of R, and in order to avoid having to update all scripts, libraries and packages (which probably would be the proper way of doing things), I just keep my old versions of R, and use the following bat-script to specify the respective version of R, to run the script in question.

~~~ batchfile
"C:\Program Files\R\R-3.4.0\bin\R.exe" CMD BATCH "C:\Path\To\myScript.R"
~~~

This script will run ``myScript.R`` and out-put the R console to a file called ``myScript.Rout`` in the same directory as the script. It will also ensure, that the script is run using R version 3.4.0.

## Outputting to Command Prompt window

The following will run the Rscript ``myScript.R`` and output the R console in the command prompt window.

~~~ batchfile
@echo on
"C:\Program Files\R\R-3.4.0\bin\Rscript.exe" "C:\Path\To\myScript.R"
~~~

Setting the ``@echo`` parameter to ``off`` should (very logically), swith output from the script off, and you will not see the console output.

## Using system PATH

A last option is to use your systems ``PATH`` environment to run your script. This will simplify your .bat file, and is the approach you usually find, but it can cause trouble if you have several versions of R installed, and you have scripts running that depend heavily on a certain version or a specific package. The approach requires R to be added to your PATH in order to work. I personally use this approach less often and mostly for quick-and-dirty jobs or testing purposes.

The approach looks like this:

~~~ batchfile
R CMD BATCH "C:\Path\To\myScript.R"
~~~

## Specifying output filename

Lastly, you can specify the file path of your output file. I usually use this, if I have a .bat file that triggers the same script several times (like fetching from APIs that tend to time out often). For the same batch files, I usually also add a bit of code to add a timestamp at the end of the ``.Rout`` file.

~~~ batchfile
For /f "tokens=1-3 delims=/-" %%a in ("%DATE%") do (set mydate=%%c%%b%%a)
For /f "tokens=1-2 delims=/:" %%a in ("%TIME%") do (set mytime=%%a%%b)

"C:\Path\To\R.exe" CMD BATCH "C:\Path\To\myScript.R" "C:\Path\To\outfile_%mydate%_%mytime%.Rout"
~~~
