# Synthesys+ Task 4.3 International Image Interoperability Framework (IIIF)

This repository is to help in deploying [IIIF](https://iiif.io/) in natural history collections as part of the [Synthesys+](http://www.synthesys.info/home.html) project funded by the European Commission. It consists of this document, which sets out recommendations for how to implement IIIF in concert with CETAF stable identifiers, plus some example files. The target audience are engineers and technicians implementing IIIF on the collections although it may be informative to others.

When a CETAF stable identifier links to a specimen record that is accompanied by a high-resolution image (or images) we want to be able to view that image in a way that is independent of the hosting institution and the technologies they use. This will enable applications to be built that can browse and combine specimen images across multiple institutions in real time, creating a truly virtual collection from the user’s perspective. IIIF is a set of API specifications used widely in the libraries and archives community for this purpose. It enables the combining of multiple views of single objects or building composite views of objects from images stored in different locations.

Synthesys+ started on 1st February 2019. Task 4.3 runs until January 2021.

There are three subtasks to Task 4.3

1. __Exemplar IIIF implementations__ involves a target of ten institutions implementing IIIF on their image servers.
1. __Integration of CETAF Identifiers into IIIF browsing__ examines the how we smoothly link between CETAF URIs and IIIF manifests to give a smooth user experience.
1. __Growing the network__ looks at how we spread the word to other collections beyond the project

__This is a working document. It will become more stable as time passes and as implementations are built based on what is written here.__ Issues & suggestions please contact the task leader [Roger Hyam](mailto:r.hyam@rbge.org.uk) or if you want to add significant material issue a pull request.

## Initial implementations 

Subtask 4.3.1 recognises that it is important to get example implementations running as quickly as possible to validate our ideas and inspire wider adoption. This is a list of institutions who aim to complete their implementations by 2021 and their current status. The order starts as arbitrary but will be updated to level of implementation. If your institution is missing or shouldn't be listed [please let me know](mailto:r.hyam@rbge.org.uk).
	
1. RBGE - Royal Botanic Garden Edinburgh, Scotland
1. BGMB - Botanic Garden and Museum Berlin, Germany
1. BGM - Meise Botanic Garden, Belgium
1. MfN - Berlin Natural History Museum, Germany
1. RMCA - Royal Museum of Central Africa, Belgium
1. NHMW - Natural History Museum Vienna, Austria
1. more to come ...

## Outline of proposed mechanism

Further details are given in sections below.

1. Institutions must implement CETAF Stable Identifiers for their specimens at level 2 or above - with a 303 redirect to RDF metadata about the specimen.
1. Institutions must implement IIIF [Image and Presentation APIs version 3](https://iiif.io/api/) or higher.
1. RDF returned for specimens should contain a link to a IIIF Manifest file for the image for that specimen.
1. The IIIF Manifest file should contain a link to the CETAF URI for the specimen.
1. It is recommended the IIIF Manifest contains some of the key Darwin Core metadata fields as name-value pairs for labels.
1. It is recommended institutions display a IIIF logo on their website next to the specimen.

## Implementation of CETAF Stable Identifiers

CETAF Stable Identifiers have three levels of implemenation

1. A simple HTTP(S) URI for each object in the collection that the institution commits to maintaining for the forseable future.
1. The URIs are well behaved semantic web URIs. This means that they will always return a "HTTP 303 See Other" redirect response to an appropriate URI. The URI redirected to will depend on content negotiation done by the server based on the accept header in the request. This comes down to a default redirect to a human readable web page or to XML encoded RDF metadata about the specimen if the "Accept: application/rdf+xml" header is present in the request. Level 2 does not specify the content of that metadata.
1. The RDF metadata contains application specific data conforming to some other standard. Currently we are working with the [CETAF Specimen Preview Profile (CSPP)](https://cetafidentifiers.biowikifarm.net/wiki/CSPP) as a common minimum set of properties - all of which are based on terms from other vocabularies.

CETAF Stable Identifiers are described further in the [CETAF URI Tester documentation](http://herbal.rbge.info/md.php?q=documentation).

## Implementation of IIIF APIs

Version 3 of the API is still in Alpha but more or less stable and is expected to be fully released in 2019.

## Linking from RDF to IIIF Manifests

Add a dc:relation to the RDF for the specimen. Here is an example:

```xml
<dc:relation>
	<rdf:Description  rdf:about="http://iiif.rbge.info/iiif/E00421509" >
		<dc:identifier rdf:resource="http://iiif.rbge.info/iiif/E00421509" />
		<dc:type rdf:resource="http://iiif.io/api/presentation/3#Manifest" />
		<dc:subject rdf:resource="https://data.rbge.org.uk/herb/E00421509" />
		<dc:format>application/ld+json</dc:format>
		<dc:description xml:lang="en">A IIIF resource for this specimen.</dc:description>
		<dc:created>2011-08-22T11:54:43Z</dc:created>
	</rdf:Description>
</dc:relation>
```

Explanation to follow

This needs to be confirmed by discussions with the IIIF community.

## Linking from IIIF Manifests to Specimens

Use the seeAlso property in the Manifest to link to the specimen.

```json
{
  "seeAlso": [
    {
      "id": "https://data.rbge.org.uk/herb/E00421509",
      "type": "Dataset",
      "format": "application/rdf+xml",
      "profile": "https://example.org/profiles/bibliographic"
    }
  ]
}
```

__ This needs to be confirmed with the 

## Recommended labels in IIIF Manifests

These are to be defined but are likely to include the main Darwin Core fields of Family, Genus, specificEpithet etc.

## Recommended display in institional catalogues

## Other possible approaches

### Using content negotiation to redirect from CETAF ID to IIIF manifest

It would be possible to link from 	


