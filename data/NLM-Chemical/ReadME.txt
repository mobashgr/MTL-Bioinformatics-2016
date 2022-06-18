README

The chemical annotation task requires  identifying which chemicals are mentioned in text. 
The NLM-Chem corpus consists of 150 full text journal articles in PubMed Central Open Access. 
These articles have been manually annotated for chemical entities in text by NLM indexers. 

Reference: Islamaj, R., Leaman, R., Kim, S. et al. NLM-Chem, a new resource for chemical 
entity recognition in PubMed full text literature. Sci Data 8, 91 (2021). 
https://doi.org/10.1038/s41597-021-00875-1

The indexing task requires identifying which chemical MeSH terms should be associated 
with the document.
All PubMed articles are indexed by human indexers with Medical Subject Heading (MeSH) terms, 
to characterize an article in the space of all articles published in the area of health, 
medicine and life sciences. These terms are available to the public and can be 
extracted/downloaded from PubMed (www.pubmed.gov). 
 
To facilitate BioCreative patricipants work, we have augmented the NLM-Chem chemical 
entity annotations with the chemical substance MeSH indexing terms that are associated with 
those articles. 

The data is presented as one file containing all 150 documents, in both XML and JSON formats.
The XML file is named: BC7T2-NLMChem-corpus_v1.BioC.xml.gz
The JSON file is named: BC7T2-NLMChem-corpus_v1.BioC.json.gz

NOTE: Previous versions of the training data contained an error in the indexing data and
should not be used

In addition, we have converted the BC5CDR and CHEMDNER data to comparable formats with the
intention that these could be used as additional training data. The BC5CDR data contains
1,500 PubMed abstracts and the CHEMDNER data contains 10,000 abstracts. Both are presented
as a single file containing all documents, again in both XML and JSON formats. Both datasets
contain indexing annotations and annotations for chemical spans. However CHEMDNER does not
contain MeSH identifiers for the chemical annotations.

Data annotations are expressed in the BioC format: http://bioc.sourceforge.net/ 

This document contains a description of the annotations listed in these files. 

There are two types of annotations: 
  1. Chemical entity annotations, used for the Chemical Indentification task
  2. MeSH indexing annotations, used for the Chemical Indexing task

Chemical Indentification task: Chemical entity annotations
---------------------------
Chemical entity annotations represent a chemical mentioned in a specific location within the text document, 
and include: an annotation identifier, the exact location in text (offset and span), the exact text mention, 
and the MeSH identifier(s) it is associated with. 
These annotations are characterized via the annotation type "Chemical" and they are associated with the passage where they are found. Here are examples of their formats:

BioC-XML:
	<annotation id="G0">
		<infon key="type">Chemical</infon>
		<infon key="identifier">MESH:D014364</infon>
		<location offset="74" length="10"/>
		<text>tryptophan</text>
	</annotation>

BioC-JSON:
{
	"id": "G0",
	"infons": {
		"type": "Chemical",
		"identifier": "MESH:D014364"
	},
	"text": "tryptophan",
	"locations": [
		{
			"offset": 74,
			"length": 10
		}
	]
}

Chemical Indexing task: MeSH Indexing annotations
-------------------------

MeSH indexing annotation represent the human indexed chemical substances for the articles in the NLM-Chem corpus. 
This type of annotation may be a chemical substance mentioned directly in the article, a MeSH tree hierarchy term that is 
an ancestor of a term mentioned directly within the article, or a MeSH term representing a concept that is implied 
within the article. 
Indexing annotations are characterized with the annotation type "MeSH_Indexing_Chemical." They are found only associated 
with the title passage within the BioC document, and include: an annotation identifier, the MeSH identifier, and the MeSH 
terminology entry term. In BioC-XML, the annotation also includes a node named text, as required by the BioC DTD, but it is empty. In BioC-JSON, the annotation also includes an empty text field and an empty list of locations, as produced by the https://github.com/bionlplab/bioc package upon converting from BioC-XML. Here are examples of their formats:

BioC-XML:
	<annotation id="MIC2">
		<infon key="type">MeSH_Indexing_Chemical</infon>
		<infon key="identifier">MESH:D007210</infon>
		<infon key="entry_term">Indoleacetic Acids</infon>
		<text/>
	</annotation>
	  
BioC-JSON:
	{
		"id": "MIC2",
		"infons": {
			"type": "MeSH_Indexing_Chemical",
			"identifier": "MESH:D007210",
			"entry_term": "Indoleacetic Acids"
		},
		"text": "",
		"locations": []
	}

Since the MeSH indexing terms are meant to represent the main topics of interest for a given article, 
it is reasonable that not all articles are indexed for chemical substances. Some NLM-Chem articles do not have 
any MeSH Indexing terms for chemical substances. This is not an error. 
