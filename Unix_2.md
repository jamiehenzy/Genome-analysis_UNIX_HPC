# Part 2: Advanced Unix
It's important to keep in mind the distinction between working on the command line _locally_ versus _remotely_. When you're working locally, you see the file structure of your own computer. When you work remotely, you see the file structure of the Explorer Cluster (EC). For Unix_1, you worked locally. For Unix_2, you'll log onto EC and work remotely.

There are three distinct directories associated with your account: home, scratch, and student. When you log onto the system, you will be in your **home** directory by default. A **scratch** directory is available for you to play around in. And you'll submit your assignments in your **student** directory. Each of the paths includes your username. For example, mine look like this:

`/home/jhenzy/
/scratch/jhenzy/
/courses/BIOL3411.202540/students/jhenzy/`

As you can see, your student directory is part of a larger courses directory. This directory also includes shared data files you can access (more on this below).

## U31: Log onto EC

`ssh <login_name>@login.explorer.northeastern.edu`

As mentioned previously, you are by default in your home directory. You also are on a node on the cluster known as the login node. To avoid interfering with the operation of the cluster, you should switch to a computing node, using the command:

`srun --pty /bin/bash`

Wait a few seconds for a prompt to appear that has a c plus a number. The c indicates you are on the computing node.

## U32: Familiarize yourself with the shared files
Navigate to our course's folders and use the `ls` command to explore the file structure:

`cd /courses/BIOL3411.202530`

**Whenever you're asked to perform an exercises with a file from any of these folders, COPY it to your own directory first.**

For this tutorial, you'll need to COPY into your student directory the three files in the shared data folder, UNIX_files. 

#### Task U32.1
Use a command to show the names of the files in the "/courses/BIOL3411.202530/data" folder. You'll need to access some of these files in this Tutorial. 

## How to Become a Unix power user

The commands that you have learned so far are essential for doing any work in Unix but they don't really let you do anything that is very useful. The following sections will introduce a few new commands that will start to show you how powerful Unix is!

---

## U33: Match making
You will often want to search files to find lines that match a certain pattern. The Unix command `grep` does this (and much more). You might already know that FASTA files (used frequently in bioinformatics) have a simple format: one header line which must start with a '>' character, followed by a DNA or protein sequence on subsequent lines. To find only those header lines in a FASTA file, we can use grep, which just requires you specify a pattern to search for, and one or more files to search.

From the data folder, copy the file "intron_IME_data.fasta" from the Arabidopsis folder into your own data folder, then perform the exercise below:

	$ grep ">" intron_IME_data.fasta

	>AT1G68260.1_i1_204_CDS 
	>AT1G68260.1_i2_457_CDS 
	>AT1G68260.1_i3_1286_CDS 
	>AT1G68260.1_i4_1464_CDS 
	.
	.
	.

This will produce lots of output which will flood past your screen. If you ever want to stop a program running in Unix, you can type Control+C (this sends an interrupt signal which should stop most Unix programs). The grep command has many different command-line options (type `man grep` to see them all) and one common option is to get `grep` to show lines that *don't* match your input pattern. You can do this with the -v option and in this example we are seeing just the sequence part of the FASTA file.

	$ grep -v ">" intron_IME_data.fasta

	GTATACACATCTCTCTACTTTCATATTTTGCATCTCTAACGAAATCGGATTCCGTCGTTG
	TGAAATTGAGTTTTCGGATTCAGTGTTGTCGAGATTCTATATCTGATTCAGTGATCTAAT 
	GATTCTGATTGAAAATCTTCGCTATTGTACAG
	GTTAGTTTTCAATGTTGCTGCTTCTGATTGTTGAAAGTGTTCATACATTTGTGAATTTAG
	TTGATAAAATCTGAACTCTGCATGATCAAAGTTACTTCTTTACTTAGTTTGACAGGGACT
	TTTTTTGTGAATGTGGTTGAGTAGAATTTAGGGCTTTGGATTAAATGTGACAAGATTTTG
	.
	.
	.

---

## U34: Your first ever Unix pipe

By now, you might be getting a bit fed up waiting for the `grep` command to finish, or you might want a cleaner way of controlling things without having to reach for Ctrl-C (also represented as \^C). Ideally, you might want to look at the output from any command in a controlled manner, i.e. you might want to use a Unix program like `less` to view the output.

This is very easy to do in Unix. You can send the output from any command to any other Unix program (as long as the second program accepts input of some sort). We do this by using what is known as a "pipe". This is implemented using the '|' character (which is a character which always seems to be on different keys depending on the keyboard that you are using). Think of the pipe as simply connecting two Unix programs. In this next example we send the output from `grep` down a pipe to the less program. Let's imagine that we just want to see lines in the input file which contain the pattern "ATGTGA" (a potential start and stop codon combined):

	$ grep "ATGTGA" intron_IME_data.fasta | less

	TTTTTTGTGAATGTGGTTGAGTAGAATTTAGGGCTTTGGATTAAATGTGACAAGATTTTG
	CTGAATGTGACTGGAAGAATGAAATGTGTTAAGATCTTGTTCGTTAAGTTTAGAGTCTTG
	GGTGGAATGAATTTATGTATCATGTGATAGCTGTTGCATTACAAGATGTAATTTTGCAAA
	GTCTATGTGATGGCCATAGCCCATAGTGACTGATAGCTCCTTACTTTGTTTTTTTTTTCT
	TTACTTGCAAAATTCCATGTGATTTTTTATATTACTTTGAAGAATTTTATAATATATTTT
	TTGCATCAAGATATGTGACATCTTCAAAAAGATAACTTGTGAGAAGACAATTATAATATG
	GTAACTTATTTATTGATTGAATCAGTAACTGTATTGTTATCATGATTTGTGAATATGTGA
	AATCTTTGTGGTGGGTCTACGATATGAGCTGTCAATATATTTTTGTTTATACATGTGATC
	GTATGTGAGCAAACGATGTCTCGTTTTCTCTCTCTCAATGATCAAGCACCTAACTTAAAT\
	.
	.
	.

Notice that you still have control of your output as you are now in the `less` program. You can use the space bar to proceed through the output. If you press the forward slash (/) key in `less`, you can then specify a search pattern. Type ATGTGA after the slash and press enter. The `less` program will highlight the location of these matches on each line. Note that `grep` matches patterns on a per line basis. So if one line ended ATG and the next line started TGA, then `grep` would not find it. To exit `less` just enter `q`.

>***Any time you run a Unix program or command that outputs a lot of text to the screen, you can instead pipe that output into the `less` program.***

---

## U35: Heads and tails

Sometimes we do not want to use `less` to see _all_ of the output from a command like grep. We might just want to see a few lines to get a feeling for what the output looks like, or just check that our program (or Unix command) is working properly. There are two useful Unix commands for doing this: head and tail. These commands show (by default) the first or last 10 lines of a file (though it is easy to specify more or fewer lines of output). So now, let's look for another pattern which might be in all the sequence files in the directory. If we didn't know whether the DNA/protein sequence in a FASTA files was in upper-case or lower-case letters, then we could use the `-i` option of `grep` which 'ignores' case when searching:

	$ grep -i ACGTC * | head
	At_proteins.fasta:TYRSPRCNSAVCSRAGSIACGTCFSPPRPGCSNNTCGAFPDNSITGWATSGEFALDVVSIQSTNGSNPGRFVKIPNLIFS
	At_proteins.fasta:FRRYGHYISSDVFRRFKGSNGNFKESLTGYAKGMLSLYEAAHLGTTKDYILQEALSFTSSHLESLAACGTCPPHLSVHIQ
	At_proteins.fasta:MAISKALIASLLISLLVLQLVQADVENSQKKNGYAKKIDCGSACVARCRLSRRPRLCHRACGTCCYRCNCVPPGTYGNYD
	At_proteins.fasta:MAVFRVLLASLLISLLVLDFVHADMVTSNDAPKIDCNSRCQERCSLSSRPNLCHRACGTCCARCNCVAPGTSGNYDKCPC
	chr1.fasta:TGTCTACTGATTTGATGTTTTCCTAAACTGTTGATTCGTTTCAGGTCAACCAATCACGTCAACGAAATTCAGGATCTTA
	chr1.fasta:TATGCTGCAAGTACCAGTCAATTTTAGTATGGGAAACTATAAACATGTATAATCAACCAATGAACACGTCAATAACCTA
	chr1.fasta:TTGAACAGCTTAGGGTGAAAATTATGATCCGTAGAGACAGCATTTAAAAGTTCCTTACGTCCACGTAAAATAATATATC
	chr1.fasta:GGGATCACGAGTCTGTTGAGTTTTCCGACGTCGCTTGGTGTTACCACTTTGTCGAACATGTGTTCTTTCTCCGGAGGTG
	chr1.fasta:CTGCAAAGGCCTACCTGTTTGTCCCTGTTACTGACAATACGTCTATGGAACCCATAAAAGGGATCAACTGGGAATTGGT
	chr1.fasta:ACGTCGAAGGGGGTAAGATTGCAGCTAATCATTTGATGAAATGGATTGGGATTCACGTGGAGGATGATCCTGATGAAGT

The `*` character acts as a wildcard meaning 'search all files in the current directory' and the `head` command restricts the total amount of output to 10 lines. Notice that the output also includes the name of the file containing the matching pattern. In this case, the `grep` command finds the ACGTC pattern in several protein sequences and several lines of the  chromosome 1 DNA sequence (we don't know how many exactly because the head command is only giving us ten lines of output).

---

## U36: Getting fancy with regular expressions

A concept that is supported by many Unix programs and also by most programming languages is that of using "regular expressions" ("regex"). These allow you to specify search patterns which are quite complex and really help restrict the huge amount of data that you might be searching for to some very specific lines of output. E.g. you might want to find lines that start with an 'ATG' and finish with 'TGA' and also have at least three AC dinucleotides in the middle:

	$ grep "^ATG.*ACACAC.*TGA$" chr1.fasta

	ATGAACCTTGTACTTCACCGGGTGCCCTCAAAGACGTTCTGCTCGGAAGGTTTGTCTTACACACTTTGATGTCAAATGA
	ATGATAGCTCAACCACGAAATGTCATTACCTGAAACCCTTAAACACACTCTACCTCAAACTTACTGGTAAAAACATTGA
	ATGCATACCTCAGTTGCATCCCGGCGCAGGGCAAGCATACCCGCTTCAACACACACTGCTTTGAGTTGAGCTCCATTGA

The `^` character is a special character that tells `grep` to only match a pattern if it occurs at the start of a line. Similarly, the `$` tells `grep` to match patterns that occur at the end of the line.

#### Task U36.1
The `.` and `*` characters are also special characters that form part of the regular expression. Try to understand how the following patterns all differ. Try using each of these these patterns with `grep` against any one of the sequence files. Can you predict which of the five patterns will generate the most matches?

	ACGT 
	AC.GT 
	AC*GT 
	AC.*GT 
	
>***The asterisk in a regular expression is similar to, but NOT the same, as the other asterisks that we have seen so far. An asterisk in a regular expression means: 'match zero or more of the preceding character or pattern'.***

Try searching for the following patterns to ensure you understand what `.` and `*` are doing:

	A...T 
	AG*T 
	A*C*G*T*

---

## U37: Counting with `grep`

Rather than showing you the lines that match a certain pattern, `grep` can also just give you a count of how many lines match. This is one of the frequently used `grep` options. Running `grep -c` simply counts how many lines match the specified pattern. It doesn't show you the lines themselves, just a number:

	$ grep -c i2 intron_IME_data.fasta 
	9785

#### Task U37.1
Count how many times each of the first three patterns from **Task U36.1** occurs in all of the sequence files (specifying `*.fasta` will allow you to specify all sequence files).

---

## U38: Regular expressions in `less`

You have seen already how you can use `less` to view files, and also to search for patterns. If you are viewing a file with `less`, you can type a forward-slash `/` character, and this allows you to then specify a pattern and it will then search for (and highlight) all matches to that pattern. Technically it is searching forward from whatever point you are at in the file. You can also type a question-mark `?` and `less` will allow you to search backwards. The real bonus is that the patterns you specify can be regular expressions.

#### Task U38.1
Try viewing a DNA sequence file (chr1.fasta or the introns file) with `less` and then searching for a pattern such as `ATCG.*TAG$`. This should make it easier to see exactly where your regular expression pattern matches. After typing a forward-slash (or a question-mark), you can press the up and down arrows to select previous searches.

---

## U39: Let me transl(iter)ate that for you

We have seen that these sequence files contain upper-case characters. What if we wanted to turn them into lower-case
characters (because maybe another bioinformatics program will only work if they are lower-case)? The Unix command `tr`
(short for transliterate) does just this – it takes one range of characters that you specify and changes them into
another range of characters. This operation alters the file so it is wise to direct the output to a new version in case you mess things up! 

	$ head -n 2 chr1.fasta

	>Chr1 dumped from ADB: Mar/14/08 12:28; last updated: 2007-12-20
	CCCTAAACCCTAAACCCTAAACCCTAAACCTCTGAATCCTTAATCCCTAAATCCCTAAATCTTTAAATCCTACATCCAT

	$ head -n 2 chr1.fasta | tr 'A-Z' 'a-z' > smallcase_chr1.fasta

	>chr1 dumped from adb: mar/14/08 12:28; last updated: 2007-12-20
	ccctaaaccctaaaccctaaaccctaaacctctgaatccttaatccctaaatccctaaatctttaaatcctacatccat

#### Task U39.1
Now that you've tested it on just the first line, alter the command to change the entire sequence into small case, directing the output into the smallcase_chr1.fasta file.

---

## U40: That's what she sed

The `tr` command let's you change a range of characters into another range. But what if you wanted to change a particular pattern into something completely different? Unix has a very powerful command called `sed` that is capable of performing a variety of text manipulations. Let's assume that you want to change the way the FASTA header looks:

	$ head -n 1 chr1.fasta 
 	
  	>Chr1 dumped from ADB: Mar/14/08 12:28; last updated: 2007-12-20

	$ head -n 1 chr1.fasta | sed 's/Chr1/Chromosome_1/' 
 
 	>Chromosome_1 dumped from ADB: Mar/14/08 12:28; last updated: 2007-12-20

The 's' part of the `sed` command puts `sed` in 'substitute' mode, where you specify one pattern (between the first two forward slashes) to be replaced by another pattern (specified between the second set of forward slashes). Note that this doesn't actually change the contents of the file, it just changes the screen output from the previous command in the pipe. We will learn later on how to send the output from a command into a new file.

---

## U41: Word up

For this section we want to work with a different type of file. It is sometimes good to get a feeling for how large a file is before you start running lots of commands against it. The `ls -l` command will tell you how big a file is, but for many purposes it is often more desirable to know how many 'lines' it has. That is because many Unix commands like `grep` and `sed` work on a line by line basis. Fortunately, there is a simple Unix command called `wc` (word count) that does this:

	$ wc At_genes.gff 
 
 	531497 4783473 39322356 At_genes.gff

The three numbers in the output above count the number of lines, words and bytes in the specified file(s). If we had run `wc -l`, the `-l` option would have shown us just the line count.

---

## U42: GFF and the art of redirection

The Arabidopsis file we looked at above is a "GFF file". This is a common file format in bioinformatics and GFF files are used to describe the location of various features on a DNA sequence. Features can be exons, genes, binding sites etc, and the sequence can be a single gene or (more commonly) an entire chromosome.

This GFF file describes all of the gene-related features from chromosome I of _A. thaliana_. We want to play around with some of this data, but don't need all of the file...just 10,000 lines will do (rather than the ~500,000 lines in the original). We will create a new (smaller) file that contains a subset of the original:

	$ head -n 10000 At_genes.gff > At_genes_subset.gff 
	$ ls -l 
	total 195360 
	-rwxrwxrwx  1 keith  staff  39322356 Jul  9 15:02 At_genes.gff 
	-rwxrwxrwx  1 keith  staff    705370 Jul 10 13:33 At_genes_subset.gff 
	-rwxrwxrwx  1 keith  staf f 17836225 Oct  9  2008 At_proteins.fasta 
	-rwxrwxrwx  1 keith  staff  30817851 May  7  2008 chr1.fasta 
	-rwxrwxrwx  1 keith  staff  11330285 Jul 10 11:11 intron_IME_data.fasta

This step introduces a new concept. Up till now we have sent the output of any command to the screen (this is the default behavior of Unix commands), or through a pipe to another program. Sometimes you just want to redirect the output into an actual file, and that is what the `>` symbol is doing, it acts as one of three [redirection operators][redirection] in Unix.

As already mentioned, the GFF file that we are working with is a standard file format in bioinformatics. For now, all you really need to know is that every GFF file has 9 fields, each separated with a tab character. There should always be some text at every position (even if it is just a '.' character). The last field often is used to store a lot of text.

[redirection]: https://en.wikipedia.org/wiki/Redirection_(Unix)
[gff]: http://www.ensembl.org/info/website/upload/gff.html

---

## U43: Not just a pipe dream

The 2nd and/or 3rd fields of a GFF file are usually used to describe some sort of biological feature. We might be interested in seeing how many different features are in our file:

	$ cut -f 3 At_genes_subset.gff | sort | uniq

	CDS 
	chromosome 
	exon 
	five_prime_UTR 
	gene 
	mRNA 
	miRNA 
	ncRNA 
	protein 
	pseudogene 
	pseudogenic_exon 
	pseudogenic_transcript 
	snoRNA 
	tRNA 
	three_prime_UTR 
	transposable_element_gene

In this example, we combine three separate Unix commands together in one go. Let's break it down (it can be useful to just run each command one at at time to see how each additional command is modifying the preceding output):

1. The `cut` command first takes the At_genes_subset.gff file and 'cuts' out just the 3rd column (as specified by the `-f` option). Luckily, the default behavior for the `cut` command is to split text files into columns based on tab characters (if the columns were separated by another character such as a comma then we would need to use another command line option to specify the comma). 
2. The `sort` command takes the output of the cut command and sorts it alphanumerically.
3. The `uniq` command (in its default format) only keeps lines that are unique to the output (otherwise you would see thousands of fields which said 'curated', 'Coding_transcript' etc.)

Now let's imagine that you might want to find which features start earliest in the chromosome sequence. The start coordinate of features is always specified by column 4 of the GFF file, so:

	$ cut -f 3,4 At_genes_subset.gff | sort -n -k 2 | head

	chromosome	1 
	exon	3631 
	five_prime_UTR	3631 
	gene	3631 
	mRNA	3631 
	CDS	3760 
	protein	3760 
	CDS	3996 
	exon	3996 
	CDS	4486

Here we first cut out just two columns of interest (3 & 4) from the GFF file. The `-f` option of the `cut` command lets us specify which columns we want to remove. The output is then sorted with the `sort` command. By default, `sort` will sort alphanumerically, rather than numerically, so we use the `-n` option to specify that we want to sort numerically. We have two columns of output at this point and we could sort based on either column. The `-k 2` specifies that we use the second column. Finally, we use the `head` command to get just the first 10 rows of output. These should be lines from the GFF file that have the lowest starting coordinate.

---

## U44: The end of the line

When you press the return/enter key on your keyboard you may think that this causes the same effect no matter what computer you are using. The visible effects of hitting this key are indeed the same...if you are in a word processor or text editor, then your cursor will move down one line. However, behind the scenes pressing enter will generate one of two different events (depending on what computer you are using). Technically speaking, pressing enter generates a newline character, which is represented internally by either a _line feed_ or _carriage return_ character (actually, Windows uses a combination of both to represent a newline). If this is all sounding confusing, well it is, and it is even more complex than we are revealing here.

The relevance of this to Unix is that you will sometimes receive a text file from someone else which looks fine on their computer, but looks unreadable in the Unix text viewer that you are using. In Unix (and other programming languages) the patterns  `\n` and `\r` can both be used to denote newlines. A common fix for this requires substituting `\r` for `\n`.

Copy the excel_data.csv file from our shared data folder to your my_data folder, then use `less` to look at the file. This is a simple 4-line file that was exported from a Mac version of Microsoft Excel. You should see that if you use `less`, then this appears as one line with the newlines replaced with ^M characters. You can convert these carriage returns into Unix-friendly line-feed characters by using the tr command like so:

	$ tr '\r' '\n' < excel_data.csv 
	sequence 1,acacagagag 
	sequence 2,acacaggggaaa 
	sequence 3,ttcacagaga 
	sequence 4,cacaccaaacac

This will convert the characters but not save the resulting output. If you want to send this output to a new file you will have to use a second redirect operator:

	$ tr '\r' '\n' < excel_data.csv > excel_data_formatted.csv

---

## U45: This one goes to 11

Finally, let's parse the Arabidopsis `intron_IME_data.fasta` file to see if we can extract a subset of sequences that match criteria based on something in the FASTA header line. Every intron sequence in this file has a header line that contains the following pieces of information:

* gene name 
* intron position in gene 
* distance of intron from transcription start site (TSS) 
* type of sequence that intron is located in (either CDS or UTR)

Let's say that we want to extract five sequences from this file that are: a) from first introns, b) in the 5' UTR, and c) closest to the TSS. Therefore we will need to look for FASTA headers that contain the text 'i1' (first intron) and also the text '5UTR'.

We can use `grep` to find header lines that match these terms, but this will not let us extract the associated sequences. The distance to the TSS is the number in the FASTA header that comes after the intron position. So we want to find the five introns which have the lowest values.

Before I show you one way of doing this in Unix, think for a moment how you would go about this if you didn't know any Unix...would it even be something you could do without manually going through a text file and selecting each sequence by eye? Note that this Unix command is so long that --- depending on how you are viewing this document --- it may appear to wrap across two lines. When you type this, it should all be on a single line:

	$ tr '\n' '@' < intron_IME_data.fasta | sed 's/>/#>/g' | tr '#' '\n' | grep "i1_.*5UTR" | sort -nk 3 -t "_" | head -n 5 | tr '@' '\n'

	>AT4G39070.1_i1_7_5UTR 
	GTGTGAAACCAAAACCAAAACAAGTCAATTTGGGGGCATTGAAAGCAAAGGAGAGAGTAG
	CTATCAAATCAAGAAAATGAGAGGAAGGAGTTAAAAAAGACAAAGGAAACCTAAGCTGCT
	TATCTATAAAGCCAACACATTATTCTTACCCTTTTGCCCACACTTATACCCCATCAACCT
	CTACATACACTCACCCACATGAGTGTCTCTACATAAACACTACTATATAGTACTGGTCCA 
	AAGGTACAAGTTGAGGGAG

	>AT5G38430.1_i1_7_5UTR 
	GCTTTTTGCCTCTTACGGTTCTCACTATATAAAGATGACAAAACCAATAGAAAAACAATT 
	AAG

	>AT1G31820.1_i1_14_5UTR 
	GTTTGTACTTCTTTACCTCTCGTAAATGTTTAGACTTTCGTATAAGGATCCAAGAATTTA
	TCTGATTGTTTTTTTTTCTTTGTTTCTTTGTGTTGATTCAG

	>AT3G12670.1_i1_18_5UTR 
	GTAGAATTCGTAAATTTCTTCTGCTCACTTTATTGTTTCGACTCATACCCGATAATCTCT
	TCTATGTTTGGTAGAGATATCTTCTCAAAGTCTTATCTTTCCTTACCGTGTTCTGTGTTT 
	TTTGATGATTTAG

	>AT1G26930.1_i1_19_5UTR 
	GTATAATATGAGAGATAGACAAATGTAAAGAAAAACACAGAGAGAAAATTAGTTTAATTA
	ATCTCTCAAATATATACAAATATTAAAACTTCTTCTTCTTCAATTACAATTCTCATTCTT
	TTTTTCTTGTTCTTATATTGTAGTTGCAAGAAAGTTAAAAGATTTTGACTTTTCTTGTTT 
	CAG

That's a long command, but it does a lot. Try to break down each step and work out what it is doing (you will need to consult the man page for some commands maybe). Notice that I use one of the other redirect operators `<` to read from a file. It took seven Unix commands to do this, but these are all relatively simple Unix commands; it is the combination of them together which makes them so powerful. One might argue that when things get this complex with Unix that it might be easier to do it in Python!

---

## Summary

Congratulations are due if you have reached this far. If you have learnt (and understood) all of the Unix commands so far then you probably will never need to learn anything more in order to do a lot of productive Unix work. But keep on dipping into the man page for all of these commands to explore them in even further detail.

The following table provides a reminder of most of the commands that we have covered so far. If you include the three, as-yet-unmentioned, commands in the last column, then you will probably be able to achieve >95% of everything that you will ever want to do in Unix (remember, you can use the `man` command to find out more about `top`, `ps`, and `kill`). The power comes from how you can use combinations of these commands.

The absolute basics	| File control |Viewing, creating, or editing files | Misc. useful commands | Power commands | Process-related commands 
:------------------:|:------------:|:----------------------------------:|:---------------------:|:--------------:|:-----------------------------:|
ls    | mv	               | less  | man    | uniq   | top   |
cd    | cp	               | head  | chmod  | sort   | ps    |
pwd   | mkdir              | tail  | source | cut    | kill  |
      | rmdir              | touch | wc	    | tr     |       |
      | rm	               | nano  |        | grep   |       |
      | \| (pipe)          |       |        | sed    |       |
      | > (write to file)  |       |        |        |       |
      | < (read from file) |       |        |        |       |
      


