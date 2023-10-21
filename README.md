# Loading Patient Genomic Information (VCFs) into Knowledge Graphs

This is a project that was started during the October 2023 Data Management for Transformer Models Hackathon (hybrid).

The overall goal of this project is to find a way to integrate or structure patient genomics information stored in VCFs as a cohort into a knowledge graph.


## Justification

We have reached the stage of being able to produce massive datasets and communicate insights rapidly. However, current clinical practice and research often struggles to keep up with this information, especially in the context of particular patients, in large part due to the ever-continuing evolution of knowledge and the quantity of that knowledge. To better equip researchers and clinicians, it is necessary to begin building frameworks that integrate our existing knowledge with patient-specific information at a cohort and individual level.

Disclaimer: This project was developed for a hackathon and is NOT at a production stage and should not be used to replace or augment clinical decisions in its current form.  


## Visual Workflow

![vcfs2kgs_flowchart](https://github.com/collaborativebioinformatics/vcfs2kgs/assets/12094168/95dadb07-1e87-493f-a5e8-ece8b02ad224)


## Dataset

As described in the flowchart above, we used the [COAD-CPTAC dataset](https://www.cbioportal.org/study/summary?id=coad_cptac_2019) as the proof-of-concept dataset. In theory, the pipeline should broadly be applicable to other similarly-structured datasets, though components of it may need to be changed (particularly which concept the patients are linked to and potentially some formatting around the loading of the data). The rest of the framework should be broadly portable, though, including the querying of the databases for unifying identifiers.


## Graph Construction

The underlying relationships within the graph are to be constructed based on cohort-specific relationships (for example, whether patients have colon adenocarcinoma or not) as well as information acquired from existing clinical knowledgebases. 

Concepts contained within the graph include:

|   Concept   |   Name                                          |   Number |
|-------------|-------------------------------------------------|----------|
|   Node Type |   Sample                                        |   93    |
|   Node Type |   Gene                                          |   TBD    |
|   Node Type |   Cancer Type                                   |   1    |
|  Edge Type  | isCancerTypeOf (connects Sample -> Cancer Type) |  93     |
|  Edge Type  | hasHugoSymbol (connects Sample -> Gene)         |  15285     |


### External Resources Used

- [BioPortal](https://www.bioontology.org/)
- Mondo Disease Ontology
  - [through BioOntology](https://bioportal.bioontology.org/ontologies/MONDO) and
  - [through their direct release](https://github.com/monarch-initiative/mondo)
- [ClinVar](https://www.ncbi.nlm.nih.gov/clinvar/) (note - this is used indirectly as the COAD-CPTAC dataset already provides these annotations)
- [HUGO Gene Nomenclature Committee Annotations](https://www.genenames.org/) to provide unique identifiers for genes



## Graph Data Format (RDF)

To store the data, we used the RDF (Resource Description Framework) data format that represents graphs as nodes and triples connecting nodes. Details on this format can be found [here](https://www.w3.org/RDF/), but in summary, the individual node types above are defined using specific ontologies or from the cohort and then the triples are pulled from information stored within the cohort (for example, which samples have mutations in what Hugo gene identifiers).

[RDFLib](https://github.com/RDFLib/rdflib) offers a very natural and accessible framework for constructing and manipulating graphs using the RDF specifications in Python. 


## Installation Instructions

The scripts for developing the model can be found in **TO BE ADDED TO GITHUB**

To install the dependencies required, you can run the following command (ideally within a virtual Python environment like Conda or virtualenv) in your terminal:

`python -m pip install rdflib requests pandas`

(Note that the current code available was developed using Python 3.10 with rdflib 7.0.0 - if the latest version you are using breaks these, you can pin your Python version and the rdflib version rather than installing the latest one). 

You will then need to provide your API key for BioOntology in the script **TO BE ADDED TO GITHUB**. To get such an API key, make an account and login to BioPortal. You will find the API key in your account settings.
