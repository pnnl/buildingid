# Frequently Asked Questions

* [What is a UBID?](#what-is-a-ubid)
* [What are the UBIDs for a given building footprint?](#what-are-the-ubids-for-a-given-building-footprint)
* [What is a UBID of unit length and width?](#what-is-a-ubid-of-unit-length-and-width)
* [Why does UBID use OLC as the underlying grid reference system?](#why-does-ubid-use-olc-as-the-underlying-grid-reference-system)
* [What is the coordinate system for UBID?](#what-is-the-coordinate-system-for-ubid)
* [What is an appropriate choice of OLC resolution?](#what-is-an-appropriate-choice-of-olc-resolution)
* [Why does UBID use the centroid of the building footprint?](#why-does-ubid-use-the-centroid-of-the-building-footprint)
* [What is the rationale for the ordering of extents in a UBID string?](#what-is-the-rationale-for-the-ordering-of-extents-in-a-ubid-string)
* [How are UBIDs used for duplicate detection?](#how-are-ubids-used-for-duplicate-detection)
* [How are UBIDs used for error detection?](#how-are-ubids-used-for-error-detection)
* [Is it possible to "round-trip" a UBID?](#is-it-possible-to-round-trip-a-ubid)

## What is a UBID?

UBIDs are transparent strings, i.e., no information is hidden.
The UBID itself is simply a string representation of the information that identifies the relative location and extent of the axis-aligned minimum bounding rectangle for a given building footprint, but not the building footprint itself.
Hence, we maintain that a UBID has no intrinsic value.
Instead, it is the assertion of the association of a UBID to another datum that has intrinsic value, e.g., the claim of ownership of a given UBID in relation to a given building footprint, which may itself be subject to other claims of ownership.
Hence, it is incorrect to use the phrase *"my UBID,"* but correct to use the phrase *"the UBID for my building footprint."*

Moreover, since extents are determined by the nature and characteristics of the ruler that is used to perform the extent measurements, the length and width of the ruler itself being determined by the OLC resolution, a variable, a given building footprint has one UBID per OLC resolution.
Hence, it is incorrect to use the phrase *"the UBID,"* but correct to use the phrase *"the UBID with the given OLC resolution."*

Given these considerations, from an ontological perspective, a UBID is an endurant, i.e., the *"UBID"* is an entity that can be observed as a complete concept, irrespective of a given point in time.
Hence, it is incorrect to use the phrase *"the UBID has changed,"* but correct to use the phrase *"the assertion of the UBID that is associated with this building footprint has changed."*
Hence, UBIDs are derived characteristics, i.e., qualities, of the building footprint (or, more generically, the geographic datum) that vary with the building footprint (or geographic datum), i.e., if the building footprint changes, then the associated UBID changes, as do any assertions of the association.
The management of assertions is beyond the scope of this document.

## What are the UBIDs for a given building footprint?

As discussed in ["What is a UBID?"](#what-is-a-ubid), a given building footprint has one UBID per OLC resolution.

![Depiction of the geometry of the building footprint for AT&amp;T Park, San Francisco, CA. (Data source for map imagery: Bing)](https://raw.githubusercontent.com/pnnl/buildingid-super/master/images/AT_and_T_Park.png)

**Figure 1.** Depiction of the geometry of the building footprint for AT&amp;T Park, San Francisco, CA. (Data source for map imagery: Bing)

A depiction of the geometry for the building footprint for AT&amp;T Park, San Francisco, CA, is given in Figure 1.
The data source for the geometry is the [open dataset of building footprint geometries for buildings in San Francisco, CA](https://data.sfgov.org/Geographic-Locations-and-Boundaries/Building-Footprints/ynuv-fyni).
The geometry is depicted as a solid-filled polygon with one hole.
The centroid of the geometry is depicted as a point.

| OLC Resolution | OLC Length | OLC Width | UBID |
|-|-|-|-|
| 0 | 180&deg; | 360&deg; | `00000000+-0-0-0-0` |
| 2 | 20&deg; | 20&deg; | `84000000+-0-0-0-0` |
| 4 | 1&deg; | 1&deg; | `849V0000+-0-0-0-0` |
| 6 | 0.05&deg; | 0.05&deg; | `849VQJ00+-0-0-0-0` |
| 8 | 0.0025&deg; | 0.0025&deg; | `849VQJH6+-0-0-1-1` |
| 10 | 0.000125&deg; | 0.000125&deg; | `849VQJH6+97-10-12-8-15` |
| 11 | 0.000025&deg; | 0.00003125&deg; | `849VQJH6+97C-52-50-41-58` |
| 12 | 0.000005&deg; | 0.0000078125&deg; | `849VQJH6+97CV-256-199-209-230` |
| 13 | 0.000001&deg; | 0.000001953125&deg;| `849VQJH6+97CVG-1279-797-1043-922` |

**Table 1.**
UBIDs of varying OLC resolution for building footprint of AT&amp;T Park, San Francisco, CA.
The unique OLC with resolution of 0 spans the entire surface of the Earth.
OLCs with even numbered resolution in the interval (0, 10] have the same length and width.
OLCs with resolution of 11 or higher have different lengths and widths, where, for successive resolutions, the length and width are divided by 5 and 4 respectively.

The UBIDs of varying OLC resolution for the building footprint for AT&amp;T Park, San Francisco, CA, are listed in Table 1.
The relative location and extent of all building footprints, including the example, is identified by the unique UBID at OLC resolution 0, `00000000+-0-0-0-0`, which spans the entire surface of the Earth.
As the OLC resolution increases, the magnitudes of the north, east, south and west extents tend to increase.

## What is a UBID of unit length and width?

Since, by definition, the OLC grid cell for the centroid of the building footprint is of unit length and width with respect to the OLC resolution, the total length and width of the axis-aligned minimum bounding rectangle identified by a UBID are equal to `1 + <north-extent> + <south-extent>` and `1 + <east-extent> + <west-extent>` respectively.
Hence, there exists a unique, one-to-one mapping from the set of OLCs to the set of UBIDs of unit length and width:
The UBID for a given OLC (`<centroid>`) is `<centroid>-0-0-0-0`.

## Why does UBID use OLC as the underlying grid reference system?

UBID uses OLC as the underlying grid reference system because OLC is an open specification.

It should also be noted that the algorithms for the UBID encoding, decoding and validation operations are compatible with any rectangular grid reference system, e.g., [Military Grid Reference System (MGRS)](https://apps.dtic.mil/docs/citations/ADA247651).
However, if another grid reference system were selected, then the syntax for UBID strings must be reconsidered such that the separator for the UBID parts does not conflict with the syntax for either the grid cells or the natural numbers, e.g., if the syntax for grid cells uses the hyphen character, then a new separator for the UBID parts must be selected.

## What is the coordinate system for UBID?

Like OLC, the coordinate system for UBID is [World Geodetic System 1984 (WGS84)](https://epsg.io/4326).

## What is an appropriate choice of OLC resolution?

The choice of OLC resolution depends upon the nature and characteristics of the objects whose relative location and extent are being identified, e.g., the OLC resolution should be selected such that the smallest objects lie within a single OLC grid cell.

The sensitivity of the measurement of extent increases with the OLC resolution.
Hence, an appropriate choice of OLC resolution is one that enables the reconciliation of small differences in building footprint geometry, where the word "small" refers to differences in length/width that are less than the length/width of the corresponding OLC grid cells at the given OLC resolution, i.e., the "tolerance" of UBID operations is one OLC grid cell at the given OLC resolution.

## Why does UBID use the centroid of the building footprint?

The centroid of the building footprint is used (instead of the centroid of the axis-aligned minimum bounding rectangle for the building footprint) because it makes the UBID encoding algorithm robust against situations where UBIDs are assigned to building footprints of irregular shape that are densely packed in a given area.

![Centroids lie within different OLC grid cells at given OLC resolution.](https://raw.githubusercontent.com/pnnl/buildingid-super/master/images/l_shaped_distinct_centroid_olc.png)

**Figure 2.**
Centroids lie within different OLC grid cells at given OLC resolution.

![Centroids lie within the same OLC grid cell at given OLC resolution.](https://raw.githubusercontent.com/pnnl/buildingid-super/master/images/l_shaped_identical_centroid_olc.png)

**Figure 3.**
Centroids lie within the same OLC grid cell at given OLC resolution.

Consider, for example, the case of an L-shaped building footprint and a rotated-L-shaped building footprint that occupy the same axis-aligned minimum bounding rectangle with respect to the same OLC resolution (Figure 2).
If the centroid of the axis-aligned minimum bounding rectangle were used, then both building footprints would be assigned the same UBID.
However, it should also be noted that the OLC resolution should itself be taken into account.
The OLC resolution should be selected such that the centroids of the two building footprints do not occupy the same OLC grid cell (Figure 3).

Calculation of the centroid of an arbitrary geometry is a feature of many software packages.
However, it should be noted that, due to differences in the implementations of certain algorithms, not all software packages return the same result when asked to compute the "centroid."
It is strongly recommended that implementations use the [JTS Topology Suite](https://www.sciencedirect.com/science/article/pii/S0198971512000890) or the [GEOS](https://trac.osgeo.org/geos) port.

## What is the rationale for the ordering of extents in a UBID string?

In the UBID string, of the 24 distinct permutations, the extents for each of the 4 cardinal direction are given in clockwise order: north, east, south, and west.
Clockwise ordering was selected in order to minimize ambiguity in the UBID specification and to minimize cultural bias (Table 2).

| Ordering | Utilization(s) |
|-|-|
| North, east, south, west | Clockwise ordering |
| North, west, south, east | Counterclockwise ordering |
| North, west, east, south | Arabic language |
| East, south, west, north | Chinese language |
| North, south, east, west | English language, Biblical Hebrew [Genesis 13:14] |

**Table 2.**
Ordering of cardinal directions with utilizations.

Another interesting example, however, is the Sanskrit language, whose set of names for the cardinal directions includes "up" and "down."
From a software developer's perspective, future versions of UBID that generalize to an arbitrary number of mutually-orthogonal dimensions may consider the English/Biblical-Hebrew/Sanskrit convention of grouping cardinal directions by axis.

In such a formalism, the extents would be ordered x<sub>1<sup>+</sup></sub>,x<sub>1<sup>-</sup></sub>,x<sub>2<sup>+</sup></sub>,x<sub>2<sup>-</sup></sub>,&hellip;,x<sub>n<sup>+</sup></sub>,x<sub>n<sup>-</sup></sub>, where x<sub>k<sup>+</sup></sub> and x<sub>k<sup>-</sup></sub> denote the positive and negative extents for the k-th axis respectively, where 1&le;k&le;n.

## How are UBIDs used for duplicate detection?

This depends on the definition of the word "duplicate."

Clearly, a pair of building footprints are "duplicates" if they are associated with identical UBIDs for all OLC resolutions; the universal qualification being necessary to guard against situations where the centroids for both building footprints lie within the same OLC grid cell at one or more, but not all, OLC resolutions.

However, since it is possible to decode any syntactically valid UBID, it is therefore possible to analyze the spatial relationships between pairs of axis-aligned minimum bounding rectangles, e.g., intersects, contains, contained-by, equals, etc.
Hence, the criteria for being a "duplicate" may be use-case-specific, e.g., at a given OLC resolution, two building footprints may be labeled as "duplicates" if the percentage overlap of their corresponding axis-aligned minimum bounding rectangles is greater than or equal to a given threshold value.

Crucially, such analyses are computationally efficient and inexpensive to perform.
Moreover, the cardinality of the subset of building footprints to be analyzed may be significantly reduced by only considering UBIDs whose OLCs have a common prefix, viz., lie within the same OLC grid cell at a given OLC resolution.

## How are UBIDs used for error detection?

The algorithm for the UBID encoding operation provides a natural setting for the implementation of error detection functionality.

Detectable "errors" include, but are not limited to:
* Rectangularity of the building footprint with respect to its axis-aligned minimum bounding rectangle is below a given threshold value.
* Area increase of the building footprint with respect to its axis-aligned minimum bounding rectangle is above a given threshold value.
* Distance from the centroid of the building footprint to the centroid of the axis-aligned minimum bounding rectangle is above a given threshold value, e.g., in decimal degrees, or in Manhattan/Chebyshev distance units with respect to a given OLC resolution.
* Cardinal direction extents, total length and/or total width are below a given threshold value with respect to a given OLC resolution.

## Is it possible to "round-trip" a UBID?

Ostensibly, yes.
To "round-trip" is to decode and then immediately encode a given UBID, receiving the original UBID as the result.
However, due to the particulars of the algorithm for the OLC encoding operation, which assumes that the distinguished point of an OLC grid cell is its southwest corner, special care must be taken to ensure that we are encoding the geographic latitude and longitude coordinates that lie within the same OLC grid cell.

When the UBID is decoded, the result includes the geographic latitude and longitude coordinates of the northeast and southwest corners of the axis-aligned minimum bounding rectangle with respect to a given OLC resolution.
Since the northeast corner of the axis-aligned minimum bounding rectangle is itself the southwest corner of a Moore-adjacent OLC grid cell, before [re-]encoding the latitude and longitude coordinates for the northeast corner must be adjusted so that they lie within the northeast OLC grid cell (and not the Moore-adjacent OLC grid cell).
Adjustment is simply subtraction by half the length and width in decimal degrees of the OLC grid cell for the given OLC resolution.

Hence, if the UBID encoding and decoding operations are denoted by `encode` and `decode` respectively, the equation `encode(decode(x)) != x` holds for all UBIDs `x`.
However, if the adjustment operation is introduced, denoted `resize`, then the equation `encode(resize(decode(x))) == x` holds for all UBIDs `x`.
