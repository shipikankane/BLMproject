# BLMproject
BUREAU OF LAND MANAGEMENT RULEMAKING DATA: 
CONTEXT AND PREPROCESSING


Introduction: 
The Bureau of Land Management (BLM), a federal agency within the US Department of Interior initiated a rulemaking procedure in 2012 to regulate Hydraulic Fracturing on federal and tribal lands. During this rulemaking process, the agency also seeks to get input from public by way of comments. BLM provides a platform for public participation by issuing a comment period during its rulemaking procedure. As the comment period begins, participants can submit comments on the given request on regulations.gov  http://www.regulations.gov. For this particular project comments for the proposed rule “Oil and Gas; Well Stimulation, Including Hydraulic Fracturing, on Federal and Indian Lands” were used.  The purpose of this project was to preprocess the files received by the Bureau of Land Management(BLM) dockets in 2012 and 2013 during the rule making process. This document preprocessing is a necessary first step to for input to the nltk(Natural Language Toolkit) parser to analyze sentiments and provide summaries. The work initiated in this project will serve as a first step in the bigger overarching goal to build a decision support system for the BLM.

Project phases:
The project was carried out in two phases. The first phase was related to acquisition of domain knowledge and the second phase was related to implementation. 

(A)	Domain Knowledge
The first phase involved systematic understanding of BLM, its rulemaking process as well as understanding of the terms in the geography domain such as hydraulic fracturing, underground injection control (UIC) program, well stimulation, well integrity. Resources on the BLMs website alongwith some EPA materials were used in this phase of the project.This phase was helpful in understanding the overall context and will be useful in the overall positioning of the research agenda. On researching about the UIC program guidelines for permit writers and relevant information, two important measures were identified that could serve as useful guidelines for the future implementation: 

1)	The zone of endangering influence (ZEI) is the lateral area in which the pressures in the injection zone may cause injection or formation fluid to migrate into Underground Sources of Drinking Water(USDW)
2)	Area of Review (AOR) that is specified in UIC Class well regulations and is submitted during Well Permit Application

(B)Implementation:
In the second phase of the project, document pre-preprocessing was implemented. First, file formats of different petition files were identified. Each organization is comment presentation style differs significantly. There are some files that present support from people in the form of table including their name, address and signature. Others presented letters addressed to the BLM endorsed by several thousands of supporters. Even with the letter kind documents, varied formatting makes it difficult to initialize sentiment analysis and summarizer. The goal of identifying different petition formats so as to make sense of the data collected and to inform the design of the overall Decision Support System to be built for the BLM.
 
This was followed by additional implementation of a tool that distinguishes between technical support documents submitted by organizations and those that were petitions in the form of letters by multiple signatures. The input to this tool is the path directory to the files. This can be initialized from within the code file. 

Data: 
The raw data was made available from regulations.gov (https://www.regulations.gov/) in the form of PDF files from comment periods in 2012 and 2013. These include technical support documents from organizations which are significantly with minimum of 48 pages and a maximum of 320 pages. The second set of documents include petitions that are essentially letters that are signed by multiple individual petitioners and submitted by an advocacy group and NGOs.  A third set is the individual comments.  In this project, the first two kinds of documents were used for classification. 

Method: 
In the implementation phase I developed a tool in python to classify documents in different buckets. It involves reading the files page by page and then uses text searching criterion to classify the PDF documents into various buckets. The tool uses PyPdf2 library to extract page content into a string format. 
Algorithm
 	for each file in the folder
		read each page
search for ‘table of contents’ or ‘appendix’ or ‘executive summary’ or ‘key findings’
		if found 
			classify as ‘technical support document’
			close file
			go to next file
		else 
			search for ‘dear’ and ‘sincerely’ on same page
			if found
				save page as new comment
				classify comment as ‘petition’ 
				close file
				go to next file
			else 
				might be tabular structure in pdf
				close file
				go to next file

Results: 
The tool was evaluated using documents included in the repository folders ‘blmdocumentexampleslong1’ and ‘blmdocumentexampleslong2’. Some additional files from the ICS Seed Grant folder on Box were also used to test the working of the tool. At this time, there are no known issues in this part of the tool. 
However, text extraction from pdf is somewhat a challenge, specifically because of the unreliability of the existing packages that parse pdfs. This problem in text extraction is limiting the classification of documents as petition. In theory and as implemented, the first step of the algorithm reads the number of pages in the files. The functions in PyPdf2 package are able to get the number of pages in the file correctly, but are unable to extract the text consistently across the files. This is limiting the correct identification of file type according to the algorithm. The algorithm was tested multiple times against different packages, but the generator used for all these data files is incomprehensible by PyPdf2. 
A significant proportion of the time was devoted to achieving optimum results using other available packages namely slate, Pdfminer, textract, and tika. However, these did not produce required results. Another option is to preprocess all pdf files as text files. This can be achieved by the CLI tool provided by Pdfminer called pdf2txt.py. This needs an input file and an output file path and name to create a text file. Subsequently, we can use the same classification algorithm on the newly created text files. However, we will lose out on the page details with this approach. 

Conclusion:
This project has provided some initial level of pre-prcessing for the overall decision Support System.
