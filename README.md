# Perseids Thematic Annotation Prototype

## Backround, Related Links and Dependencies
This repository contains documentation and code for the Thematic Annotation Prototype.  This prototype was developed for the Perseids [Vortex Workshop](http://sites.tufts.edu/perseids/meetings/vortex-april-16-17-2015/)

The prototype digital edition is deployed at [http://www.perseids.org/sites/vortex/digitaledition/tlg0012.tlg001.perseus-grc1.de.html](http://www.perseids.org/sites/vortex/digitaledition/tlg0012.tlg001.perseus-grc1.de.html)

The original workflow requirements and design are described at [https://github.com/perseids-project/perseids_docs/issues/234](https://github.com/perseids-project/perseids_docs/issues/234)

A detailed description of the use case can be found at [https://github.com/perseids-project/thematic-annotation-prototype/raw/master/papers/PerseidsScholarlyCommunicationsWhitePaper.docx](https://github.com/perseids-project/thematic-annotation-prototype/raw/master/papers/PerseidsScholarlyCommunicationsWhitePaper.docx)

End user instructions can be found at [http://sites.tufts.edu/perseids/instructions/workshops/vortex-workshop-instructions/](http://sites.tufts.edu/perseids/instructions/workshops/vortex-workshop-instructions/)

The prototype tagset used for the annotations is kept in the arethusa-configs GitHub repo at [https://github.com/alpheios-project/arethusa-configs/blob/master/configs/arethusa.relation/vortex.json](https://github.com/alpheios-project/arethusa-configs/blob/master/configs/arethusa.relation/vortex.json)

The code for the template annotation input form for Arethusa is in the [https://github.com/perseids-project/perseids-client-apps/blob/master/app/templates/treebank/theme.html](perseids-client-apps) repository

## Perseids Platform Setup
1. Create a CITE image collection to host the data. At the time of the workshop we were using the [Angular ImgCollect app](https://github.com/perseids-project/imgcollect_angular) and [JackSON service](https://github.com/perseids-project/jackson) deployed at [http://www.perseids.org/jackson/apps/imgcollect/#/collections](http://www.perseids.org/jackson/apps/imgcollect/#/collections)
2. Create a CTS inventory to hold the Pseudo work to be used for the Image Annotations. At the time of the workshop we used the [vortex inventory](https://github.com/perseids-project/cts-inventories/blob/master/vortex.xml).
3. Load the new CTS inventory into the SoSOL CTS service and add it to the SoSOL environment configuration.
4. Make the Vortex [thematic annotation tagset](https://github.com/alpheios-project/arethusa-configs/blob/master/configs/arethusa.relation/vortex.json) available to the Arethusa environment by deploying it at the aux_data_configs path specified in the Arethusa configuration.
    1. At the time of the workshop, these were deployed at [http://services.perseids.org/arethusa-configs](http://services.perseids.org/arethusa-configs). See related item [https://github.com/alpheios-project/arethusa/issues/661](https://github.com/alpheios-project/arethusa/issues/661)
5. Deploy the [thematic annotation input form](http://www.perseids.org/apps/thematic) with the perseids-client-apps. 

## Data Prerequisites for Annotation
1. Well-formed, CTS-compliant EpiDoc XML document containing the target CTS Passage of the primary source text being annotated
2. Well-formed, CTS-compliant EpiDoc XML document containing the CTS Passage of the translation of the primary source text
3. CTS Inventory  Metadata for the primary source text and its translation
4. The image or images to be annotated.

## Perseids Platform Steps to load a set of texts and images for Annotation
1. Add the Primary Source text, the translation, and inventory metadata to a CTS inventory and load the inventory and xml texts into the CTS Repository instance serving the Perseids annotation environment.  At the time of the workshop, this was the [annotsrc inventory](https://github.com/perseids-project/cts-inventories/blob/master/annotsrc.xml) served by the  [https://sosol.perseids.org/exist/rest/db/xq/CTS.xq?request=GetCapabilities&inv=annotsrc](sosol.perseids.org eXist repository)
2. Load the images and metadata into the CITE Image Collection designated for hosting the images.  At the time of the workshop this was the [urn:cite:perseus:vorteximg](http://www.perseids.org/jackson/apps/imgcollect/#/collection/urn:cite:perseus:vorteximg) collection.
    1. the `crm:P138_represents` field of the CITE image collection object should be set to the CTS URN of the pseudo-work designated for the TEI XML Image Annotations files, i.e. `urn:cts:pdltmp:vortex.img001`. 

## Creating the Digital Edition Dissemination
1. Prerequisite: annotations created and submitted to the Perseids community board as described at [http://sites.tufts.edu/perseids/instructions/workshops/vortex-workshop-instructions/](http://sites.tufts.edu/perseids/instructions/workshops/vortex-workshop-instructions/)
2. Clone this repository
3. Install oXygen and import transformation scenarios
    * in oXygen, Options, Import Global Options and select scenarios.xml (found at the root of this repository)
    * This will load the XQuery and XSLT transformation scenarios listed below into your oXygen environment
    * The scenarios configure parameter values as input to the alignannotations.xquery and teiDriver.xsl files
4. Download the annotation data from the board and put them into the xml directory of this repository
    * Translation Alignment
    * Thematic Annotation
    * Image Annotations (TEI XML)
5. Call the CTS API to retrieve the primary source passage and translation that correspond to the target of the annotations and save the output in the xml directory of this repository.
    * You will need to strip the CTS API request/reply data from the output, leaving only the TEI xml
    * If you don't have a source translation (e.g. if the annotator created their own for the alignment) you can use the alignment-to-tei.xsl transform located in the src/xslt directory of this repo to create a TEI xml file from the translation alignment annotation.
6. Apply naming conventions to all files:
    * Source passage: `textgroup.work.edition.xml` (e.g. `tlg0012.tlg001.perseus-grc1.xml`)
    * Translated passage: `textgroup.work.translation.xml` (e.g. `tlg0012.tlg001.perseus-eng1.xml`)
    * Translation alignment: `textgroup.work.edition.translation.align.xml` (e.g. `tlg0012.tlg001.perseus-grc1.perseus-eng1.align.xml`)
    * Images: `textgroup.work.edition.images.xml` (e.g. `tlg0012.tlg001.perseus-grc1.images.xml`)
    * Thematic Annotations: `textgroup.work.edition.tb.xml` (e.g. `tlg0012.tlg001.perseus-grc1.tb.xml`)
7. Open the primary source text xml file
8. run alignannotations-greekDemo or alignannotations-latinDemo XQuery transformation scenario
    1. Document, Transformation, Configure Transformation Scenario
    2. Select XML Transformation With XQuery transformation type
    3. Select alignannotations-greekDemo (or alignannotations-latinDemo)
    4. Make sure oXygen is set to output results as a file (click Edit, Output tab and make sure Evaluate as Sequence is unchecked)
    5. Update the parameters to have the correct values for e_work, e_translation and e_edition (i.e. it must correspond to the values from the urn of the target text)
    6. Click Transform Now
9. Open translated xml
10. run reversealignannotations-greekDemo or reversealignannotations-latinDemo XQuery transformation scenario
    1. Same instructions as above for alignannotations-greekDemo/alignannotations-latinDemo
11. Open newly created digital edition xml
    this the file created in the previous step, it should be in the digitaleditions directory and is named according to the convention `textgroup.work.edition.de.xml`
12. run transformtodisplay-greekDemo or transformtodisplay-latinDemo XSLT Transformation scenario
    1. Update the following parameters to have the correct values:
        1. `{http://data.perseus.org/namespaces/teida}urn` (the base urn for the target passage)
        1. `{http://data.perseus.org/namespaces/teida}translationFile` (path to the file containing the translation text)
13. TODO run xxx to transform the open annotation rdf to turtle
14. load the resulting html file in your browser
    * `digitaledition/textgroup.work.edition.de.html`

