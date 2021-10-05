# Introduction

The purpose of this project is to test an applicant's coding and software design
abilities with an actual current project confronting the Core LMI team at Emsi.
It is meant to take less than twenty hours to complete.

# Project Story

The Quarterly Census of Employment and Wages (QCEW) is an important data source
for Emsi. We base nearly all our derived employment estimates on cleaned and
unsuppressed data from QCEW. In the past, we have filtered out any QCEW data
that is not published at the county, state, or national level, but have
recently realized that we could improve the constraints on our unsuppression
models if we retained the data that QCEW publishes by metropolitan area.

Metropolitan areas consist of counties but often do not fit within a single
state, so we are unable to fit them into a single hierarchical structure
with the data we currently gather from QCEW.

The goal of this project is to define alternate geography hierarchies that
make use of as much metropolitan area data as possible from QCEW. The
hierarchical structure of the output files can be visualized:

```
           US000
          /     \
      USMSA     USNMS
     /   |        |
  USCMS  |     counties (non-metropolitan)
    |    |
  CS###  |
     \   |
      C####
        |
     counties (metropolitan)
```

The first few lines of the file will likely be:

```
child,parent,name
US000,US000,U.S. TOTAL
USMSA,US000,U.S. Metropolitan Statistical Areas (combined)
USNMS,US000,U.S. Nonmetropolitan Area Counties (combined)
USCMS,USMSA,U.S. Combined Statistical Areas (combined)
CS###,USCMS,<CSA name here>
C####,CS###,<MSA name here>
C####,USMSA,<MSA name here>
C####,USNMS,<MSA name here>
#####,C####,<county name here>
etc.
etc.
```

# Project Requirements

* Seven output files:
    * 2003 MSA definitions with
        * 1993 counties
        * 2002 counties
        * 2008 counties
        * 2009 counties
    * 2013 MSA definitions with
        * 2009 counties
        * 2014 counties
        * 2015 counties
* Output files should have this header: child,parent,name
* The following geography codes in the raw file should fit into the hierarchy:
    * `US000` (US total)
    * `USMSA` (Sum of all metropolitan areas)
    * `USNMS` (Sum of all non-metropolitan counties)
    * `USCMS` (Sum of all combined metro areas)
    * `CS###` (Combined metro areas)
    * `C####` (Metro areas - in older files they might be formatted `M####`)
    * `#####` (counties, including `##999` "Unknown Or Undefined" counties)
* Do not include:
    * `##000` (states)
    * `##996` (Overseas Locations)
    * `##997` (Multicounty, Not Statewide)
    * `##998` (Out-of-State)

# Recommendations

I would start by creating the current definition file by downloading and parsing
the current QCEW county-MSA crosswalk linked below. Then you can use the county
version mappings to create the  other output files that are based on the older
county definitions.

The 2003-based definitions will be harder, but you may be able to re-create
QCEW's current crosswalk using the 2013 MSA delineation file and then apply
the same transformations to the 2003 MSA delineation file to create an old
crosswalk.

# Resources

* MSA Delineation Files: https://www.census.gov/geographies/reference-files/time-series/demo/metro-micro/delineation-files.html
* QCEW Area Guide: https://www.bls.gov/cew/classifications/areas/area-guide.htm
* QCEW Classifications List: https://www.bls.gov/cew/classifications/
* QCEW current county-MSA crosswalk: https://www.bls.gov/cew/classifications/areas/county-msa-csa-crosswalk.htm
* QCEW old area definitions: https://www.bls.gov/cew/classifications/areas/old-area-titles.htm
* QCEW county definitions: dims/dim_county_####.csv
* QCEW county version mappings: maps/map_####_####.csv

# Implementation Notes

* Your code can be written in the language of your choice.
* Your code must run on Debian Stretch. Include a README.md that describes how
  to compile and run your code.
* Elegance and performance will both be taken into consideration.
* If you find any requirements to be ambiguous, go with your best guess and
  document your decision. We are not intentionally being ambiguous, but we do
  want to evaluate your persistence in the face of roadblocks.
* If you reach twenty hours of work on the project, take note of where you are
  and what you were planning to do next, and submit your work in progress.

# Submission Process

Create a repository in Github and email us a link.
