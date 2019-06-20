# UBID Specification

This document is the specification for the Unique Building Identifier (UBID).

## General UBID Syntax

UBIDs are used to identify the relative location and extent of axis-aligned minimum bounding rectangles on the surface of the Earth.
Having identified a relative location and extent, the system may perform a variety of operations, as might be characterized by such words as "encode," "decode" and "validate."

### The main parts of UBIDs

In general, UBIDs are written as follows:

`<centroid>-<north-extent>-<east-extent>-<south-extent>-<west-extent>`

A UBID contains the string encoding of the [Open Location Code (OLC)](https://github.com/google/open-location-code) grid cell, viz., the OLC string, for the centroid of the building footprint (`<centroid>`), followed by a hyphen, and then the north, east, south and west extents (`<north-extent>`, `<east-extent>`, `<south-extent>` and `<west-extent>`), separated by hyphens.

The syntax for OLC strings is that of the [OLC specification](https://github.com/google/open-location-code/blob/master/docs/olc_definition.adoc).
The syntax for the north, east, south and west extents is a sequence of digits, representing a natural number.
The UBID parts are separated by hyphens so as to not conflict with the syntax for OLC strings or natural numbers.

## UBID Operations

In this section, we give the algorithms for encoding, decoding and validating UBIDs.

### Encoding UBIDs

For a given building footprint, the minimum data requirements for encoding a UBID are seven numbers: the geographic latitude and longitude coordinates of the northeast (NE) and southwest (SW) corners of its axis-aligned minimum bounding rectangle; the geographic latitude and longitude coordinates of its center of mass (viz., centroid; C); and the resolution of the OLC grid reference system (viz., OLC resolution).

The algorithm for encoding a UBID with a given OLC resolution is as follows:
1. Encode C as an OLC string with the given OLC resolution (`<centroid>`) and then immediately decode the result to obtain the corresponding OLC grid cell.
2. Encode NE and SW as OLC strings with the given OLC resolution and then immediately decode the results to obtain the corresponding OLC grid cells.
3. Measure the distance in decimal degrees from the northern edge of C to the northern edge of NE and divide by the length in decimal degrees of C (`<north-extent>`).
4. Measure the distance in decimal degrees from the eastern edge of C to the eastern edge of NE and divide by the width in decimal degrees of C (`<east-extent>`).
5. Measure the distance in decimal degrees from the southern edge of C to the southern edge of SW and divide by the length in decimal degrees of C (`<south-extent>`).
6. Measure the distance in decimal degrees from the western edge of C to the western edge of SW and divide by the width in decimal degrees of C (`<west-extent>`).
7. Write the UBID string.

### Decoding UBIDs

When a UBID with a given OLC resolution is decoded, the result is geographic latitude and longitude coordinates for the northeast (NE) and southwest (SW) corners of the OLC grid cells that contain the NE and SW corners of the axis-aligned minimum bounding rectangle for the building footprint; and the OLC grid cell that contains the centroid of the building footprint (C).

The algorithm for decoding a UBID is as follows:
1. Split the UBID string into its five hyphen-separated parts.
2. Decode `<centroid>` to yield the OLC grid cell for C.
3. Multiply the length in decimal degrees of C by `<north-extent>` and add to the latitude of the northern edge of C to yield the latitude of the northern edge of NE.
4. Multiply the width in decimal degrees of C by `<east-extent>` and add to the latitude of the eastern edge of C to yield the longitude of the eastern edge of NE.
5. Multiply the length in decimal degrees of C by `<south-extent>` and subtract from the latitude of the southern edge of C to yield the latitude of the southern edge of SW.
6. Multiply the width in decimal degrees of C by `<west-extent>` and subtract from the longitude of the western edge of C to yield the longitude of the western edge of SW.

### Validating UBIDs

By definition, a UBID is syntactically valid if the OLC string for the centroid of the building footprint (`<centroid>`) is syntactically valid and if all extents are syntactically valid, and a UBID is semantically valid if the OLC string for the centroid of the building footprint (`<centroid>`) is semantically valid and if all extents are non-negative.
