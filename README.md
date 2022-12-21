This is a set of JSON-LD files populated with data from ThermoML (XML) files documented at
https://trc.nist.gov/ThermoML/ and available at https://doi.org/10.18434/mds2-2422. The available files can be
downloaded from the links below (there is no need to clone the whole repository):
- [fpe_221221.zip](jsonld/fpe_221221.zip): set of 20,454 files
- [jct_221221.zip](jsonld/jct_221221.zip): set of 36,165 files
- [jced_221221.zip](jsonld/jced_221221.zip): set of 57,232 files
- [ijt_221221.zip](jsonld/ijt_221221.zip): set of 1,541 files
- [tca_221221.zip](jsonld/tca_221221.zip): set of 8,011 files

### Rationale
As part of an [NSF project](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1835643) the set of ThermoML files 
created by the NIST Thermodynamics Research Center (TRC) in Boulder, CO, USA was ingested into a [MySQL database](https://github.com/ChalkLab/Dataset-NIST-TRC-MySQL) 
as a stepping stone to subsequent conversion of the data into JSON-LD files in the [SciData framework](https://stuchalk.github.io/scidata/) 
format. This dataset is made available as an example of translating an XML data model into Resource Description 
Format (RDF) as JSON-LD (an RDF encoding) the SciData framework.  It is also used to exemplify the following tenets 
of good data management: data modeling, unique identifiers (foreign keys in database tables) and best practices 
for findable, interoperable, accessible, and reusable ([FAIR](https://www.go-fair.org/)) data.

### Description
Data in the MySQL database created by ingestion of the ThermoML XML files for this dataset was processed with a PHP
function ([function scidata {}](https://github.com/ChalkLab/SciData_TRC/blob/main/app/Controller/DatasetsController.php)) 
as part of a CakePHP application.  In order to do the conversion a CakePHP model 
([Scidata.php](https://github.com/ChalkLab/SciData_TRC/blob/main/app/Model/Scidata.php)) was used to standardize the 
formatting of the output JSON-LD files in accordance with the design patterns of the SciData framework. In order to
annotate the experimental about with its appropriate metadata, yet manage the size of the resultant JSON-LD files,
the content of one JSON-LD document is "data about one chemical system (pure or a mixture) from one research paper".
The original ThermoML files capture all the data from one research paper, so each one with be converted to as many
JSON-LD files as there are different chemical systems reported in each ThermoML file.

To add semantic meaning to the data in the files, context files were created to for the metadata
added in each JSON-LD file.  The context files are generic for data for chemical substances, chemicals (samples), 
and mixtures. Additionally, a crosswalks table was developed in the MySQL database to add the semantic annotation of
quantities reported in the files.  These were created in a [ThermoML ontology](https://stuchalk.github.io/scidata/ontology/thermo.owl) 
which is nothing more that a placeholder as definitions for the quantities is not currently available (note the disclaimer below).

### Visualizing Triples and Quads
The JSON-LD files may be new to many coming to this repository, but if you visualize them in a web browser (use plugins
like [JSONView](https://jsonview.com/) (for Firefox, Chrome and Edge) or [JSON Lite](https://github.com/lauriro/json-lite)
(for Firefox and Chrome)) or in a text editor (for example [TextMate](https://macromates.com/) (Mac only) or 
[Sublime Text](https://www.sublimetext.com/)) you will see that they are human-readable. RDF on the other hand is
simpler in its construct but harder to visualize in aggregation as it can be a large set of data.

In short, RDF is small statements of knowledge using the [subject predicate object pattern](https://en.wikipedia.org/wiki/Semantic_triple).
A quick example is:

(Subject)`Stuart Chalk` (Predicate) [`foaf:mbox`](http://xmlns.com/foaf/0.1/#term_mbox) (Object) `schalk@unf.edu` 
(in this case a literal value).

When a JSON-LD file contains the `@graph` keyword (as all the files in this repository do) the data inside
that JSON object is said to be 'part of a named graph'.  In this instance, the triples are turned into 'quads'
where the name of the graph is added at the end of the triple.  In order to allow users to see what the quads 
from this data look like, the quads were saved into MySQL tables (this is not normally done) and exported to 
the following SQL files which can be imported into MySQL and searched.
- [fpe_quads_221221.sql.zip](quads/fpe_quads_221221.sql.zip): table of 13,465,010 quads
- [jct_quads_221221.sql.zip](quads/jct_quads_221221.sql.zip): table of 27,432,649 quads
- [jced_quads_221221.sql.zip](quads/jced_quads_221221.sql.zip): table of 42,729,338 quads
- [ijt_quads_221221.sql.zip](quads/ijt_quads_221221.sql.zip): table of 1,288,848 quads
- [tca_quads_221221.sql.zip](quads/tca_quads_221221.sql.zip): table of 4,910,813 quads

This activity also provided validation
of the JSON-LD files by:
1. verifying they were valid JSON-LD (no errors on conversion from JSON-LD to RDF)
2. visualizing the predicates, to ensure correct annotation
3. creating statistics that confirmed complete conversion of all files in each journal based set

Statistics on the predicates generated in this dataset can be found in this [Excel](quads/predicates_by_journal.xlsx) 
spreadsheet. The 122,403 JSON-LD files are converted to 89,826,658 quads.

### Additional Data Availability
This dataset has also been made available at the UNF repository for SciData, https://scidata.unf.edu.  The
datasets may be searched by going to the following endpoints: [FPE](https://scidata.unf.edu/tranche/trc/fpe/), 
[JCT](https://scidata.unf.edu/tranche/trc/jct/), [JCED](https://scidata.unf.edu/tranche/trc/jced/), 
[IJT](https://scidata.unf.edu/tranche/trc/ijt/), [TCA](https://scidata.unf.edu/tranche/trc/tca/) 
(a comprehensive search still under development).  A SPARQL endpoint to search the data is also under development.

### Disclaimer
This work was completed independently of the creators of the NIST ThermoML dataset.  It was also done
independently of the International Union of Pure and Applied Chemistry ([IUPAC](https://iupac.org/))
and therefore does not have any endorsement from IUPAC, even though the developer is an active member
of the IUPAC and is currently a member of the IUPAC Committee on Publications and Cheminformatics
Data Standards ([CPCDS](https://iupac.org/body/024/)).
