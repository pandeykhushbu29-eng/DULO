DULO
Open Spreadsheet Engine Specification

Version 0.1 Draft

Licensed under the DULO PACK LICENSE (DPL)

Chapter 1 — Introduction
1.1 Purpose

DULO is an open, implementation-independent specification defining the architecture, behavior, interfaces, and conformance requirements of a modern spreadsheet engine.

Unlike complete spreadsheet applications, DULO specifies the engine responsible for workbook management, formula evaluation, dependency tracking, storage, and extensibility. It intentionally does not define any particular graphical user interface, allowing multiple independent implementations across desktop, web, mobile, embedded, command-line, and cloud environments.

The primary objective of DULO is to provide a consistent foundation upon which spreadsheet software can be built while remaining portable, extensible, deterministic, and future-proof.

1.2 Scope

This specification defines:

Workbook architecture.
Worksheet organization.
Cell storage and addressing.
Formula language and semantics.
Calculation engine behavior.
Data typing.
Dependency tracking.
File format.
Plugin interfaces.
AI integration interfaces.
Performance requirements.
Security requirements.
Conformance requirements.

This specification does not define:

User interface layouts.
Toolbar appearance.
Menu systems.
Operating-system integration.
Rendering engines.
Specific programming languages.
1.3 Design Philosophy

DULO is built upon the following principles:

Platform Independence

Implementations SHALL operate independently of operating systems, user interfaces, and hardware architectures.

Deterministic Behaviour

The same workbook SHALL always produce identical calculation results under identical inputs and implementation versions.

Extensibility

Every major subsystem SHOULD permit future extensions without requiring incompatible architectural redesign.

Performance

The engine SHALL prioritize efficient memory utilization and scalable calculation for large workbooks.

Interoperability

Multiple independent implementations SHALL be capable of exchanging DULO workbooks without loss of supported information.

Headless Operation

The specification assumes no graphical interface.

A DULO engine MAY execute entirely without windows, graphics, or user interaction.

1.4 Terminology

The following terms are normative throughout this specification.

Workbook

The highest-level container representing an entire spreadsheet document.

Worksheet

An individual spreadsheet page contained within a workbook.

Cell

The smallest addressable storage unit capable of containing data, formulas, metadata, or formatting.

Formula

An expression evaluated by the calculation engine.

Dependency

A relationship whereby one formula references another cell.

Calculation Graph

The directed graph representing dependencies between formula cells.

Implementation

Any software conforming to this specification.

Conformance

The degree to which an implementation satisfies mandatory requirements defined herein.

1.5 Normative Keywords

The keywords

SHALL
SHALL NOT
MUST
MUST NOT
SHOULD
SHOULD NOT
MAY

are to be interpreted as normative requirement levels throughout this specification.

1.6 Conformance Levels

Implementations SHALL declare one of the following conformance levels.

Level	Description
Core	Supports all mandatory engine functionality.
Extended	Supports optional extensions defined by this specification.
Full	Supports all mandatory and optional functionality.
1.7 Guiding Goals

DULO aims to provide:

predictable calculations,
open interoperability,
modern extensibility,
AI compatibility,
high performance,
implementation simplicity,
long-term maintainability.
1.8 Non-Goals

DULO does not attempt to:

replace every existing spreadsheet format,
mandate one graphical interface,
enforce one programming language,
require proprietary technologies,
depend upon cloud services.
1.9 Chapter Overview

This specification is divided into fourteen chapters followed by three appendices.

Each chapter defines a major subsystem of the DULO architecture.

1.10 Conformance Checklist

An implementation claiming DULO compliance:

✓ SHALL implement a workbook abstraction.

✓ SHALL support worksheets.

✓ SHALL implement cells.

✓ SHALL implement formulas.

✓ SHALL follow normative terminology.

✓ SHALL declare a conformance level.

✓ SHALL remain implementation independent.

Chapter 2 — Architecture
2.1 Architectural Overview

The DULO architecture is divided into modular subsystems.

Each subsystem SHALL operate independently while exposing well-defined interfaces to other components.

A conforming implementation MAY combine subsystems internally but SHALL preserve externally observable behavior defined by this specification.

2.2 Core Architecture

A conceptual DULO engine consists of the following components:

               Workbook Manager
                      │
      ┌───────────────┼───────────────┐
      │               │               │
Worksheet      Storage Manager    Metadata
      │
      ▼
Cell Manager
      │
      ▼
Formula Engine
      │
      ▼
Calculation Engine
      │
      ▼
Dependency Graph
      │
      ▼
Result Cache
      │
      ▼
Plugin & AI Interface

Implementations MAY optimize or reorganize internal structures provided observable behavior remains conformant.

2.3 Engine Lifecycle

A DULO engine progresses through the following conceptual states:

Initialization
Workbook Creation or Loading
Validation
Dependency Graph Construction
Formula Compilation
Calculation
Event Processing
Persistence
Shutdown

Implementations MAY introduce additional internal states.

2.4 Component Responsibilities
Workbook Manager

Responsible for:

workbook creation,
workbook loading,
workbook saving,
workbook metadata,
workbook validation.
Worksheet Manager

Responsible for:

worksheet ordering,
worksheet creation,
worksheet deletion,
worksheet visibility,
worksheet metadata.
Cell Manager

Responsible for:

cell allocation,
addressing,
formatting references,
value storage,
formula storage,
cell history (if implemented).
Formula Engine

Responsible for:

parsing,
validation,
optimization,
execution preparation.
Calculation Engine

Responsible for:

dependency analysis,
recalculation,
circular reference detection,
cache invalidation,
deterministic evaluation.
Storage Manager

Responsible for:

serialization,
deserialization,
compression,
snapshots,
recovery.
Extension Manager

Responsible for:

plugin loading,
package management,
custom functions,
AI tool interfaces.
2.5 Layered Design

The architecture SHALL conceptually separate:

Applications
      ▲
API Layer
      ▲
Extension Layer
      ▲
Calculation Engine
      ▲
Formula Engine
      ▲
Workbook Model
      ▲
Storage Layer

Implementations MAY merge layers internally.

2.6 Threading Model

The specification does not require a specific threading implementation.

However:

calculations SHOULD support parallel execution where safe,
storage operations MAY execute asynchronously,
plugins SHALL NOT violate calculation determinism.
2.7 Error Isolation

Subsystem failures SHALL remain isolated where reasonably possible.

A plugin failure SHALL NOT corrupt workbook state.

Formula evaluation errors SHALL propagate according to Chapter 6.

2.8 Future Compatibility

Reserved architectural extension points SHALL permit future additions without breaking compliant implementations.

Unknown optional features SHALL be ignored safely unless explicitly required.

2.9 Design Objectives

The architecture SHALL prioritize:

modularity,
scalability,
deterministic behavior,
extensibility,
portability,
maintainability.
2.10 Conformance Checklist

A conforming implementation:

✓ SHALL implement the workbook model.

✓ SHALL implement the formula engine.

✓ SHALL implement the calculation engine.

✓ SHALL isolate storage functionality.

✓ SHALL expose defined subsystem behavior.

✓ SHALL preserve deterministic calculations.

✓ SHALL support future-compatible extension points.

# Chapter 3 — Workbook Model

## 3.1 Overview

The Workbook Model defines the logical structure of every DULO workbook.

A workbook SHALL be the root object of every DULO document and SHALL contain one or more worksheets.

Every workbook SHALL be self-contained and SHALL preserve all information necessary for deterministic calculation and presentation.

---

## 3.2 Workbook

A workbook SHALL contain:

• Workbook Identifier
• Workbook Metadata
• Worksheets
• Named Objects
• Workbook Settings
• Global Styles
• Shared Resources
• Extension Data

Implementations MAY store additional implementation-specific metadata provided interoperability is not affected.

---

## 3.3 Workbook Identifier

Every workbook SHALL possess a unique identifier.

The identifier SHALL remain constant unless intentionally regenerated.

The identifier SHOULD be globally unique.

---

## 3.4 Workbook Metadata

Metadata MAY include:

Title

Author

Description

Keywords

Creation Timestamp

Last Modification Timestamp

Revision Number

Language

Default Locale

Default Currency

License Information

Custom Metadata

Unknown metadata fields SHALL be preserved whenever possible.

---

## 3.5 Workbook Settings

Settings MAY include:

Default Font

Default Font Size

Calculation Mode

Date System

Default Decimal Separator

Thousands Separator

Measurement Units

Default Time Zone

Compatibility Mode

Reserved Future Fields

---

## 3.6 Worksheets

Every workbook SHALL contain at least one worksheet.

Worksheets SHALL be uniquely identifiable within the workbook.

Worksheet names SHALL be unique.

Worksheet ordering SHALL be preserved.

---

## 3.7 Worksheet Properties

Each worksheet SHALL define:

Worksheet Identifier

Worksheet Name

Visibility State

Protection State

Dimensions

Metadata

Display Settings

Calculation Cache

Extension Data

---

## 3.8 Worksheet States

A worksheet MAY exist in one of the following states:

Visible

Hidden

Very Hidden

Archived (optional)

Protected

Read Only

Implementations MAY define additional internal states.

---

## 3.9 Worksheet Dimensions

Worksheets SHALL support:

Rows

Columns

Cells

Named Regions

Tables

Charts

Images

Comments

Hyperlinks

Shapes

Embedded Objects

---

## 3.10 Cell Addressing

Every cell SHALL possess a unique address.

Example:

A1

B12

AA32

ZZ999

Addressing SHALL remain deterministic.

Alternative addressing systems MAY be implemented internally.

---

## 3.11 Cell Ownership

Every cell SHALL belong to exactly one worksheet.

Cells SHALL NOT exist independently.

---

## 3.12 Cell Lifetime

Cells MAY be:

Created

Modified

Referenced

Deleted

Restored

Imported

Copied

Moved

The engine SHALL preserve workbook consistency during all operations.

---

## 3.13 Named Objects

Named objects SHALL permit symbolic references.

Examples include:

Named Cells

Named Ranges

Named Constants

Named Tables

Named Functions

Named Charts

Names SHALL remain unique within their scope.

---

## 3.14 Named Range Scope

A named range MAY exist as:

Workbook Scope

Worksheet Scope

Private Scope (optional)

---

## 3.15 Tables

Tables SHALL represent structured rectangular datasets.

Tables MAY define:

Headers

Filters

Calculated Columns

Totals

Metadata

Relationships

---

## 3.16 Shared Resources

Implementations SHOULD avoid duplication through shared resources.

Examples:

Fonts

Styles

Images

Color Palettes

Themes

Formula Packages

---

## 3.17 Workbook Integrity

The engine SHALL ensure:

Unique worksheet names.

Valid references.

Consistent metadata.

No orphaned objects.

No duplicate identifiers.

---

## 3.18 Future Compatibility

Unknown workbook objects SHALL be preserved where possible.

Unknown optional features SHALL NOT invalidate compliant workbooks.

---

## 3.19 Conformance Checklist

✓ Workbooks SHALL contain worksheets.

✓ Worksheets SHALL possess unique identifiers.

✓ Cell addresses SHALL remain deterministic.

✓ Named objects SHALL be supported.

✓ Workbook metadata SHALL be preserved.

✓ Worksheet ordering SHALL remain stable.

✓ Shared resources SHALL remain consistent.

✓ Workbook integrity SHALL be maintained.

# Chapter 4 — Data Model

## 4.1 Overview

The Data Model defines every value type recognized by the DULO engine.

Every stored value SHALL belong to exactly one defined data type.

Implementations MAY internally optimize storage but SHALL preserve externally observable behaviour.

---

## 4.2 Primitive Types

The following primitive types are mandatory.

Number

Text

Boolean

Empty

Error

Date

Time

Timestamp

Duration

---

## 4.3 Number

Numbers SHALL support:

Signed Integers

Unsigned Integers

Floating Point

High Precision Decimal

Scientific Notation

Infinity (optional)

NaN (optional)

Precision requirements SHALL be implementation-defined.

---

## 4.4 Text

Text SHALL support Unicode.

The engine SHALL preserve character order.

Implementations SHOULD support normalization.

Maximum string length MAY be implementation-defined.

---

## 4.5 Boolean

Boolean values SHALL be:

TRUE

FALSE

No additional boolean states SHALL exist.

---

## 4.6 Empty

Empty cells SHALL differ from:

Zero

False

Empty String

Null Error

The calculation engine SHALL distinguish empty values.

---

## 4.7 Error Values

Mandatory errors include:

#REF!

#VALUE!

#DIV/0!

#NAME?

#NUM!

#NULL!

#CIRC!

#PARSE!

Implementations MAY introduce additional errors.

---

## 4.8 Date

Dates SHALL represent calendar dates independent of formatting.

Supported calendar systems MAY be implementation-defined.

---

## 4.9 Time

Time values SHALL represent time-of-day.

Precision SHALL be implementation-defined.

---

## 4.10 Timestamp

A timestamp combines:

Date

Time

Time Zone (optional)

---

## 4.11 Duration

Duration values SHALL represent measurable spans of time.

Examples:

5 seconds

3 hours

18 days

2 years

---

## 4.12 Rich Text

Rich text MAY include:

Font

Bold

Italic

Underline

Color

Hyperlinks

Embedded Styles

---

## 4.13 Binary Objects

Cells MAY reference binary objects.

Examples:

Images

Icons

Attachments

Audio

Video

Binary objects SHALL remain external to formula evaluation.

---

## 4.14 Formatting

Formatting SHALL remain logically separate from stored values.

Changing formatting SHALL NOT modify data.

---

## 4.15 Cell State

Every cell SHALL maintain:

Stored Value

Displayed Value

Data Type

Formatting

Formula

Metadata

Dependencies

History (optional)

---

## 4.16 Metadata

Cells MAY contain metadata.

Examples:

Comments

Notes

Tags

Validation Rules

Permissions

Audit Information

---

## 4.17 Nullability

Data types SHALL define null behaviour.

Null SHALL remain distinguishable from Empty.

---

## 4.18 Type Conversion

Conversions MAY occur automatically where permitted.

Unsafe conversions SHOULD produce errors.

Implicit conversion rules SHALL remain deterministic.

---

## 4.19 Future Data Types

Future implementations MAY introduce additional data types.

Unknown types SHALL be safely ignored or preserved.

---

## 4.20 Conformance Checklist

✓ Primitive data types SHALL be implemented.

✓ Error values SHALL be distinguishable.

✓ Formatting SHALL remain independent.

✓ Unicode SHALL be supported.

✓ Empty values SHALL remain distinct.

✓ Data conversions SHALL be deterministic.

✓ Cell metadata SHALL remain consistent.

# Chapter 5 — Formula Language

## 5.1 Overview

The Formula Language defines the syntax, semantics, evaluation rules, and execution model of all formulas recognized by a DULO implementation.

A formula SHALL represent a deterministic expression evaluated by the Calculation Engine.

Every conforming implementation SHALL evaluate formulas according to the rules defined within this chapter.

---

## 5.2 Formula Declaration

A formula SHALL begin with the Formula Prefix.

Default Formula Prefix:

=

Alternative prefixes MAY be supported by implementations but SHALL NOT alter standard behavior.

Examples:

=A1+B1

=SUM(A1:A20)

=IF(B1>100,"PASS","FAIL")

---

## 5.3 Formula Grammar

A formula SHALL consist of one or more of the following:

• Literals
• References
• Operators
• Function Calls
• Arrays
• Parenthesized Expressions
• Named Objects
• Constants

Nested expressions SHALL be supported.

---

## 5.4 Literals

Supported literal categories include:

Numeric

Text

Boolean

Date

Time

Timestamp

Duration

Array

Null

Error

Literal syntax SHALL remain deterministic.

---

## 5.5 Operators

Mandatory arithmetic operators:

+

-

*

/

%

^

Mandatory comparison operators:

=

<>

<

>

<=

>=

Mandatory logical operators:

AND

OR

NOT

Operator precedence SHALL follow Appendix A.

---

## 5.6 Parentheses

Parentheses SHALL override default operator precedence.

Nested parentheses SHALL be unlimited unless implementation limits are documented.

---

## 5.7 Cell References

Formulas SHALL support references to:

Single Cells

Cell Ranges

Named Cells

Named Ranges

Tables

Worksheet References

Workbook References

Future external references MAY be added.

---

## 5.8 Relative References

Relative references SHALL update when copied or moved.

Example:

=A1+B1

Copied downward becomes:

=A2+B2

---

## 5.9 Absolute References

Absolute references SHALL remain unchanged during movement.

Example:

=$A$1

---

## 5.10 Mixed References

Mixed references SHALL preserve fixed rows or columns.

Examples:

A$1

$A1

---

## 5.11 Range Expressions

Supported examples:

A1:A10

B2:E30

Entire Row

Entire Column

Named Range

Table Column

---

## 5.12 Array Expressions

The Formula Language SHALL support arrays.

Arrays MAY be:

Static

Dynamic

Computed

Nested

---

## 5.13 Dynamic Arrays

Dynamic arrays MAY automatically expand.

Expansion SHALL NOT overwrite occupied cells.

Overflow SHALL generate an appropriate error.

---

## 5.14 Named Objects

Named objects SHALL behave identically to direct references.

Names SHALL resolve before evaluation.

---

## 5.15 Function Invocation

Functions SHALL use parentheses.

Examples:

SUM()

AVERAGE()

ABS()

Custom()

Nested calls SHALL be permitted.

---

## 5.16 Function Categories

Mandatory categories:

Mathematics

Logical

Statistical

Text

Lookup

Financial

Date & Time

Array

Information

Database

Engineering

Future categories MAY be added.

---

## 5.17 Error Propagation

Errors SHALL propagate unless explicitly handled.

Example:

=1/0

Produces:

#DIV/0!

Nested expressions SHALL preserve originating errors.

---

## 5.18 Implicit Conversion

Permitted automatic conversions include:

Integer → Decimal

Date → Timestamp

Boolean → Numeric (optional)

Unsafe conversions SHALL generate errors.

---

## 5.19 Formula Parsing

Parsing SHALL occur before evaluation.

Invalid syntax SHALL produce parser errors.

Implementations MAY cache parsed expressions.

---

## 5.20 Formula Compilation

Implementations MAY compile formulas into internal representations.

Compilation SHALL NOT alter observable behavior.

---

## 5.21 Formula Caching

Cached expressions MAY be reused.

Cache invalidation SHALL occur after dependency changes.

---

## 5.22 Circular References

Circular references SHALL be detected.

Implementations MAY support iterative calculation as an optional feature.

---

## 5.23 Determinism

Formula evaluation SHALL remain deterministic.

Given identical inputs,

identical outputs SHALL always be produced.

---

## 5.24 Reserved Keywords

Keywords reserved by DULO SHALL NOT be used as identifiers.

Future specifications MAY extend this list.

---

## 5.25 Future Compatibility

Unknown functions SHALL produce defined errors.

Unknown operators SHALL fail safely.

---

## 5.26 Conformance Checklist

✓ Formula prefix implemented.

✓ Operators supported.

✓ Relative references supported.

✓ Absolute references supported.

✓ Dynamic arrays supported (if implemented).

✓ Error propagation deterministic.

✓ Parsing deterministic.

✓ Nested expressions supported.

✓ Circular references detected.

✓ Formula behavior conforms to Appendix A.

# Chapter 6 — Calculation Engine

## 6.1 Overview

The Calculation Engine evaluates formulas, constructs dependency graphs, manages recalculation, and ensures deterministic spreadsheet execution.

This chapter defines the normative behaviour of all calculation operations.

---

## 6.2 Calculation Graph

Every workbook SHALL construct a directed dependency graph.

Each formula cell SHALL represent a graph node.

Dependencies SHALL form directed edges.

---

## 6.3 Dependency Discovery

Dependencies SHALL be discovered during parsing.

Implementations SHALL update dependency graphs whenever formulas change.

---

## 6.4 Topological Ordering

Independent calculations SHALL be ordered using dependency analysis.

Execution order SHALL remain deterministic.

---

## 6.5 Incremental Recalculation

Only affected cells SHALL be recalculated whenever possible.

Full workbook recalculation SHALL remain optional.

---

## 6.6 Lazy Evaluation

Implementations MAY delay evaluation until values are requested.

Observable behaviour SHALL remain identical.

---

## 6.7 Parallel Evaluation

Independent calculations MAY execute concurrently.

Parallel execution SHALL preserve deterministic results.

---

## 6.8 Calculation Modes

Supported modes:

Automatic

Manual

On Demand

Background

Custom

---

## 6.9 Dirty Cells

Modified cells SHALL be marked dirty.

Dependent cells SHALL inherit dirty state.

---

## 6.10 Cache Management

Implementations MAY cache:

Formula Results

Compiled Expressions

Dependency Information

Caches SHALL invalidate correctly.

---

## 6.11 Circular References

Cycles SHALL be detected.

Supported responses include:

Immediate Error

Iterative Calculation

User Notification

Maximum Iterations

Maximum Error Threshold

---

## 6.12 Transaction Model

Implementations SHOULD support transactional calculation.

Operations:

Begin

Validate

Commit

Rollback

Transactions SHALL preserve workbook consistency.

---

## 6.13 Recovery

Interrupted calculations SHALL recover gracefully.

Partial corruption SHALL be minimized.

---

## 6.14 Event Generation

Calculation events MAY include:

Calculation Started

Calculation Finished

Cell Updated

Formula Changed

Dependency Changed

Workbook Recalculated

---

## 6.15 Volatile Functions

Volatile functions SHALL recalculate whenever required.

Examples include:

NOW()

TODAY()

RAND()

Additional volatile functions MAY exist.

---

## 6.16 Numerical Stability

Implementations SHOULD minimize accumulated numerical error.

Floating-point behaviour SHALL be documented.

---

## 6.17 Memory Optimisation

Implementations MAY:

Compress caches

Discard temporary structures

Virtualize inactive regions

Observable behaviour SHALL remain unchanged.

---

## 6.18 Performance Objectives

The engine SHOULD optimize:

Dependency analysis

Memory usage

Parallel execution

Cache locality

Large workbook performance

---

## 6.19 Thread Safety

Concurrent evaluation SHALL NOT corrupt workbook state.

Shared resources SHALL remain synchronized.

---

## 6.20 Failure Handling

Failures SHALL produce deterministic error states.

Unexpected termination SHALL preserve workbook integrity whenever possible.

---

## 6.21 Future Extensions

Future specifications MAY introduce:

GPU acceleration

Distributed calculation

Remote execution

Vectorized evaluation

Incremental compilation

---

## 6.22 Conformance Checklist

✓ Dependency graph implemented.

✓ Incremental recalculation supported.

✓ Circular references detected.

✓ Dirty cell tracking implemented.

✓ Cache invalidation correct.

✓ Deterministic execution guaranteed.

✓ Thread safety maintained.

✓ Failure recovery implemented.

# Chapter 7 — Storage & File Format

## 7.1 Overview

This chapter defines the normative storage model used by DULO implementations.

The Storage Layer SHALL preserve workbook integrity, deterministic behaviour, compatibility, and recoverability independent of implementation language or operating system.

Implementations MAY choose any internal storage architecture provided externally observable behaviour remains conformant.

---

## 7.2 Storage Objectives

The storage subsystem SHALL provide:

• Deterministic serialization

• Fast loading

• Fast saving

• Incremental saving

• Streaming support

• Crash recovery

• Forward compatibility

• Backward compatibility where applicable

---

## 7.3 Workbook Representation

A workbook SHALL consist of logical components.

Mandatory components include:

Workbook Metadata

Worksheet Collection

Cell Storage

Styles

Named Objects

Calculation Information

Extension Data

Reserved Fields

---

## 7.4 Physical Format

This specification does not mandate a single physical file format.

Implementations MAY choose:

Binary

Compressed Archive

Directory Structure

Database

Memory Image

Cloud Object

Provided logical behaviour remains identical.

---

## 7.5 Serialization

Serialization SHALL preserve:

Values

Formulas

Formatting

Metadata

Dependencies

Named Objects

Protection Settings

Extension Data

No mandatory information SHALL be lost.

---

## 7.6 Deserialization

Loading SHALL reconstruct:

Workbook

Worksheets

Dependency Graph

Calculation Cache

Metadata

Extension Objects

Unknown objects SHALL be preserved whenever possible.

---

## 7.7 Versioning

Every workbook SHALL define:

Major Version

Minor Version

Revision

Future Version Fields

Unknown versions SHALL fail safely.

---

## 7.8 Snapshots

Implementations SHOULD support snapshots.

Snapshots SHALL permit restoration to previous workbook states.

Snapshots MAY be:

Manual

Automatic

Scheduled

Transactional

---

## 7.9 Incremental Saving

Implementations MAY save only modified regions.

Observable workbook contents SHALL remain identical to a complete save.

---

## 7.10 Recovery

The storage layer SHOULD minimize corruption.

Recovery mechanisms MAY include:

Journal Files

Write-Ahead Logging

Atomic Replacement

Temporary Files

Checksums

Recovery SHALL preserve maximum possible data.

---

## 7.11 Compression

Compression MAY be applied.

Compression SHALL NOT alter workbook semantics.

---

## 7.12 Streaming

Large workbooks MAY support streaming.

Streaming SHALL permit partial loading.

Invisible workbook regions SHOULD remain unloaded where practical.

---

## 7.13 Integrity

Integrity SHALL verify:

Workbook structure

Metadata

Worksheet references

Named objects

Checksums

Optional digital signatures

---

## 7.14 Reserved Fields

Reserved storage regions SHALL remain untouched by unknown implementations.

---

## 7.15 Compatibility

Future implementations SHALL preserve unknown extension objects whenever practical.

---

## 7.16 Conformance Checklist

✓ Serialization implemented.

✓ Deserialization implemented.

✓ Workbook integrity preserved.

✓ Version information stored.

✓ Recovery supported.

✓ Streaming behaviour defined.

✓ Reserved fields preserved.

✓ Unknown objects safely handled.

# Chapter 8 — Spreadsheet Features

## 8.1 Overview

This chapter defines user-visible spreadsheet behaviour independent of graphical presentation.

All features described herein SHALL remain accessible through implementation APIs regardless of interface.

---

## 8.2 Cell Formatting

Cells MAY define formatting independent of stored values.

Formatting SHALL include:

Font

Size

Weight

Style

Underline

Foreground Colour

Background Colour

Alignment

Borders

Number Format

---

## 8.3 Styles

Styles MAY be shared between cells.

Modifying a shared style SHALL update all dependent cells.

Implementations MAY duplicate styles internally.

---

## 8.4 Conditional Formatting

Conditional formatting SHALL evaluate expressions.

Supported actions MAY include:

Colour Changes

Icons

Data Bars

Gradients

Custom Formatting

Conditions SHALL automatically recalculate.

---

## 8.5 Data Validation

Cells MAY define validation rules.

Examples include:

Numeric Range

Text Length

Date Limits

Custom Formula

Named List

Invalid input SHALL generate deterministic behaviour.

---

## 8.6 Comments

Cells MAY contain comments.

Comments SHALL remain independent of formulas.

---

## 8.7 Notes

Implementations MAY distinguish notes from comments.

---

## 8.8 Hyperlinks

Cells MAY reference:

URLs

Workbook Locations

Worksheet Locations

Named Objects

Future resource types

---

## 8.9 Images

Worksheets MAY contain images.

Images SHALL remain independent of formula evaluation.

---

## 8.10 Embedded Objects

Implementations MAY support:

Audio

Video

Documents

Custom Objects

Unknown objects SHALL remain preserved.

---

## 8.11 Sorting

Sorting SHALL support:

Ascending

Descending

Multi-column

Custom Order

Stable Sorting

---

## 8.12 Filtering

Filtering SHALL support:

Text

Number

Date

Boolean

Custom Expressions

Filtering SHALL NOT modify stored data.

---

## 8.13 Freeze Panes

Viewport freezing SHALL NOT alter workbook structure.

---

## 8.14 Merge Cells

Merged cells SHALL preserve:

Original Cell Addresses

Formatting

Content

Merge metadata

---

## 8.15 Hidden Objects

Rows

Columns

Worksheets

Objects

MAY be hidden without deletion.

---

## 8.16 Search

Implementations SHOULD support:

Find

Replace

Regular Expressions (optional)

Whole Workbook Search

---

## 8.17 Clipboard Operations

Mandatory operations:

Copy

Cut

Paste

Paste Special

Duplicate

Clipboard operations SHALL preserve supported metadata.

---

## 8.18 Undo / Redo

Implementations SHOULD maintain operation history.

Undo SHALL restore previous workbook state.

Redo SHALL restore reversed operations.

---

## 8.19 Accessibility

Implementations SHOULD expose accessibility metadata.

---

## 8.20 Conformance Checklist

✓ Formatting supported.

✓ Styles implemented.

✓ Validation implemented.

✓ Comments preserved.

✓ Sorting deterministic.

✓ Filtering deterministic.

✓ Clipboard operations implemented.

✓ Undo/Redo supported.

# Chapter 9 — Charts & Visualization

## 9.1 Overview

This chapter defines the normative visualization model of DULO.

Charts SHALL represent graphical interpretations of workbook data.

The visualization subsystem SHALL remain independent from the Calculation Engine while remaining synchronized with workbook changes.

---

## 9.2 Chart Objects

A chart SHALL be represented as an independent workbook object.

Charts SHALL reference one or more data sources.

Chart objects SHALL support metadata, formatting, layout, and extension information.

---

## 9.3 Chart Types

Mandatory chart categories include:

Bar

Column

Line

Area

Pie

Scatter

Bubble

Radar

Histogram

Additional chart types MAY be implemented.

---

## 9.4 Data Sources

Charts SHALL support:

Single Cell

Cell Range

Named Range

Tables

Calculated Arrays

Dynamic Arrays

Future data source types MAY be introduced.

---

## 9.5 Dynamic Behaviour

Charts SHALL automatically update whenever referenced data changes.

Manual refresh MAY additionally be supported.

---

## 9.6 Chart Layout

Chart layout SHALL define:

Position

Dimensions

Margins

Legend Position

Axis Placement

Title

Subtitle

Data Labels

Layout SHALL remain independent from workbook calculations.

---

## 9.7 Axes

Supported axis categories include:

Numeric

Category

Date

Time

Logarithmic

Custom

Axis scaling SHALL remain deterministic.

---

## 9.8 Series

Each chart SHALL contain one or more series.

Series SHALL define:

Name

Values

Formatting

Visibility

Ordering

Metadata

---

## 9.9 Legends

Charts MAY display legends.

Legend ordering SHALL correspond to displayed series.

---

## 9.10 Titles

Charts MAY define:

Title

Subtitle

Axis Titles

Series Labels

Titles SHALL support Unicode text.

---

## 9.11 Formatting

Chart formatting MAY include:

Colours

Gradients

Borders

Transparency

Fonts

Background

Themes

Formatting SHALL NOT alter source data.

---

## 9.12 Interaction

Implementations MAY support:

Selection

Zoom

Pan

Tooltips

Highlighting

Export

Interactive behaviour SHALL remain optional.

---

## 9.13 Export

Charts MAY be exported independently.

Supported export formats MAY include:

PNG

SVG

PDF

Bitmap

Vector

Future formats MAY be added.

---

## 9.14 Future Compatibility

Unknown chart objects SHALL be preserved whenever possible.

---

## 9.15 Conformance Checklist

✓ Charts implemented.

✓ Dynamic updates supported.

✓ Data references deterministic.

✓ Formatting independent.

✓ Export behaviour defined.

✓ Unknown chart metadata preserved.

# Chapter 10 — Extension System

## 10.1 Overview

The Extension System enables implementations to extend DULO without modifying the core specification.

Extensions SHALL remain isolated from the engine core.

Unknown extensions SHALL NOT invalidate compliant workbooks.

---

## 10.2 Extension Types

Supported extension categories include:

Plugins

Packages

Custom Functions

Automation

Importers

Exporters

Language Bindings

AI Providers

Future extension types MAY be defined.

---

## 10.3 Plugin Model

Plugins SHALL execute through defined interfaces.

Plugins SHALL NOT directly modify internal engine structures.

All communication SHALL occur through public APIs.

---

## 10.4 Plugin Lifecycle

A plugin MAY progress through:

Installation

Registration

Initialization

Execution

Suspension

Termination

Removal

---

## 10.5 Plugin Metadata

Every plugin SHALL define:

Identifier

Name

Version

Author

Description

License

Required Permissions

Compatibility Information

---

## 10.6 Extension Packages

Packages MAY provide:

Functions

Themes

Charts

Templates

Localization

Data Providers

Validators

Packages SHALL remain independently installable.

---

## 10.7 Custom Functions

Extensions MAY register custom worksheet functions.

Custom functions SHALL behave identically to standard functions unless otherwise documented.

---

## 10.8 Events

The engine SHALL expose events.

Examples include:

WorkbookOpened

WorkbookSaved

WorksheetCreated

WorksheetDeleted

CellChanged

FormulaChanged

CalculationStarted

CalculationFinished

PluginLoaded

PluginRemoved

Future events MAY be added.

---

## 10.9 Automation

Extensions MAY automate workbook behaviour.

Automation SHALL preserve workbook integrity.

---

## 10.10 SDK

Implementations SHOULD expose a Software Development Kit.

The SDK SHOULD provide:

Workbook API

Worksheet API

Cell API

Formula API

Storage API

Chart API

Plugin API

---

## 10.11 Language Bindings

Implementations MAY expose bindings for:

Rust

C

C++

Python

Java

JavaScript

Go

.NET

Future languages MAY be supported.

---

## 10.12 Isolation

Extension failures SHALL NOT corrupt workbook state.

Engine recovery SHALL remain deterministic.

---

## 10.13 Compatibility

Extensions SHALL declare compatibility requirements.

Unknown capabilities SHALL fail safely.

---

## 10.14 Future Compatibility

Reserved extension points SHALL permit future specification revisions.

---

## 10.15 Conformance Checklist

✓ Plugin model implemented.

✓ Event system implemented.

✓ SDK documented.

✓ Extension isolation maintained.

✓ Compatibility declared.

✓ Future extension points preserved.

# Chapter 11 — AI Integration

## 11.1 Overview

Artificial Intelligence integration is an optional subsystem of DULO.

The DULO engine SHALL remain fully functional without any AI implementation.

This chapter defines a standardized interface through which AI systems MAY interact with conforming DULO implementations.

No AI provider is mandated by this specification.

---

## 11.2 Design Goals

The AI subsystem SHALL:

• Remain provider independent.

• Preserve workbook integrity.

• Operate through public APIs.

• Never directly modify engine internals.

• Support local and remote AI providers.

• Permit future provider implementations.

---

## 11.3 AI Providers

Supported provider categories MAY include:

Local Models

Remote APIs

Enterprise Models

Embedded Models

Custom Providers

Future provider types MAY be added.

---

## 11.4 Provider Registration

Each provider SHALL define:

Identifier

Display Name

Version

Capabilities

Authentication Requirements

Supported Models

Configuration Schema

---

## 11.5 AI Sessions

AI interactions SHALL occur within sessions.

A session MAY define:

Current Workbook

Selected Worksheet

Current Selection

Conversation Context

Provider State

Temporary Memory

---

## 11.6 Context Access

The engine SHALL expose context through controlled interfaces.

Examples include:

Selected Cells

Named Ranges

Workbook Metadata

Visible Worksheets

Chart Metadata

Formula Definitions

Formatting Information

Dependency Graph

Implementations SHALL prevent unauthorized access.

---

## 11.7 Tool Invocation

AI systems SHALL interact with workbooks through standardized tools.

Mandatory tool categories include:

Workbook Tools

Worksheet Tools

Cell Tools

Formula Tools

Formatting Tools

Chart Tools

Storage Tools

Inspection Tools

Future tools MAY be added.

---

## 11.8 Workbook Tools

Examples include:

Create Workbook

Open Workbook

Save Workbook

Duplicate Workbook

Rename Workbook

Delete Workbook

Close Workbook

---

## 11.9 Worksheet Tools

Examples include:

Create Worksheet

Rename Worksheet

Delete Worksheet

Duplicate Worksheet

Hide Worksheet

Protect Worksheet

Move Worksheet

---

## 11.10 Cell Tools

Examples include:

Read Cell

Write Cell

Insert Cell

Delete Cell

Copy Cell

Move Cell

Merge Cells

Split Cells

Validate Cell

---

## 11.11 Formula Tools

Examples include:

Create Formula

Edit Formula

Explain Formula

Validate Formula

Optimize Formula

Trace Dependencies

Find Circular References

Generate Formula

---

## 11.12 Analysis Tools

Examples include:

Summarize Data

Detect Trends

Detect Outliers

Generate Insights

Recommend Charts

Recommend Tables

Generate Pivot Structures

Future analytical tools MAY be added.

---

## 11.13 Permissions

Implementations SHOULD permit configurable permissions.

Permissions MAY include:

Read

Write

Delete

Execute

Analyze

Export

Import

Automation

---

## 11.14 Safety

AI providers SHALL NOT bypass engine validation.

Every operation SHALL be validated before execution.

Rejected operations SHALL produce deterministic responses.

---

## 11.15 Provider Independence

Implementations SHALL NOT require any specific provider.

Changing providers SHALL NOT alter workbook behaviour.

---

## 11.16 Future Compatibility

Future AI technologies MAY implement this interface without requiring changes to the workbook model.

---

## 11.17 Conformance Checklist

✓ Provider-independent architecture.

✓ Tool interface implemented.

✓ Session management supported.

✓ Permission system implemented.

✓ Context isolation preserved.

✓ Workbook integrity maintained.

✓ AI remains optional.

# Chapter 12 — Performance

## 12.1 Overview

The Performance chapter defines behavioural expectations for efficient DULO implementations.

Performance optimizations SHALL NOT modify observable workbook behaviour.

---

## 12.2 Objectives

Implementations SHOULD optimize:

Memory Usage

Calculation Speed

Loading

Saving

Rendering Support

Streaming

Concurrency

Large Workbook Handling

---

## 12.3 Memory Model

Implementations MAY optimize memory through:

Shared Objects

Compression

Object Pools

Lazy Allocation

Reference Counting

Virtual Memory

---

## 12.4 Large Workbook Support

Implementations SHOULD support workbooks substantially larger than available physical memory where practical.

Large workbook techniques MAY include:

Streaming

Virtual Storage

Paging

Compression

Deferred Loading

---

## 12.5 Lazy Loading

Objects MAY remain unloaded until first access.

Lazy loading SHALL preserve observable behaviour.

---

## 12.6 Virtualization

Rows

Columns

Worksheets

Charts

Objects

MAY be virtualized.

---

## 12.7 Caching

Implementations MAY cache:

Formula Results

Styles

Metadata

Charts

Dependency Graphs

Parsed Expressions

Caches SHALL invalidate correctly.

---

## 12.8 Parallelism

Independent operations MAY execute concurrently.

Observable behaviour SHALL remain deterministic.

---

## 12.9 Background Operations

Examples include:

Calculation

Loading

Saving

Validation

Compression

Background execution SHALL remain interruptible.

---

## 12.10 Benchmarking

Implementations SHOULD publish benchmark methodology.

Recommended benchmark categories include:

Calculation

Loading

Saving

Memory

Large Workbook

Plugin Performance

---

## 12.11 Scalability

Implementations SHOULD scale efficiently with:

Workbook Size

Worksheet Count

Formula Count

Dependency Count

Chart Count

Plugin Count

---

## 12.12 Performance Metrics

Recommended metrics include:

Execution Time

Peak Memory

Average Memory

Cache Hit Rate

Calculation Throughput

Serialization Speed

---

## 12.13 Future Optimizations

Future revisions MAY introduce:

GPU Calculation

Distributed Calculation

SIMD Acceleration

Hardware Offloading

Cloud Execution

---

## 12.14 Conformance Checklist

✓ Memory optimizations documented.

✓ Cache behaviour deterministic.

✓ Parallel execution safe.

✓ Lazy loading supported where implemented.

✓ Benchmark methodology documented.

✓ Scalability maintained.

# Chapter 13 — Security

## 13.1 Overview

This chapter defines the security principles and requirements applicable to conforming DULO implementations.

Security mechanisms SHALL preserve workbook integrity, protect user data, isolate untrusted components, and prevent unauthorized modification of workbook state.

The DULO specification intentionally separates workbook security from operating-system security.

---

## 13.2 Security Objectives

Every implementation SHOULD prioritize:

Integrity

Availability

Confidentiality

Recoverability

Deterministic Behaviour

Safe Extensibility

---

## 13.3 Trust Model

DULO defines three conceptual trust domains:

Core Engine

Trusted Extensions

Untrusted Extensions

Implementations MAY define additional trust levels.

---

## 13.4 Workbook Integrity

The engine SHALL preserve workbook integrity during:

Loading

Saving

Calculation

Plugin Execution

Import

Export

Unexpected Termination

---

## 13.5 Validation

Every externally supplied workbook SHALL undergo validation prior to execution.

Validation SHALL include:

Workbook Structure

Worksheet References

Formula Parsing

Metadata Consistency

Extension Metadata

Reserved Fields

Malformed workbooks SHALL fail safely.

---

## 13.6 Formula Safety

Formula evaluation SHALL NOT permit arbitrary execution outside the defined Formula Language.

Implementations SHALL distinguish between:

Formula Evaluation

Extension Execution

Automation

---

## 13.7 Extension Isolation

Extensions SHALL execute using public interfaces only.

Extensions SHALL NOT modify internal engine structures directly.

Extension failures SHALL remain isolated.

---

## 13.8 Permission Model

Implementations SHOULD expose configurable permissions.

Examples include:

Workbook Read

Workbook Write

Worksheet Management

Formula Creation

Extension Installation

AI Tool Access

Import

Export

Automation

Permissions SHALL be independently configurable.

---

## 13.9 Secure Storage

Stored workbooks SHOULD support:

Integrity Verification

Checksums

Optional Digital Signatures

Optional Encryption

Secure Metadata

Future storage protections MAY be introduced.

---

## 13.10 Recovery

Following abnormal termination,

implementations SHOULD recover the maximum possible workbook state.

Recovery SHALL preserve structural consistency.

---

## 13.11 Error Handling

Security failures SHALL produce deterministic error behaviour.

Unexpected behaviour SHALL NOT silently corrupt workbook contents.

---

## 13.12 Logging

Implementations MAY maintain security logs.

Security logs SHOULD remain separate from workbook contents.

Logs MAY include:

Validation Failures

Plugin Errors

Recovery Events

Permission Violations

---

## 13.13 Future Compatibility

Future revisions MAY define:

Cryptographic Signatures

Remote Verification

Collaborative Security

Secure Synchronization

Additional Permission Classes

---

## 13.14 Conformance Checklist

✓ Workbook validation implemented.

✓ Formula execution isolated.

✓ Extension isolation maintained.

✓ Permission system documented.

✓ Recovery behaviour defined.

✓ Integrity verification supported.

✓ Security failures deterministic.

# Chapter 14 — Conformance

## 14.1 Overview

This chapter defines the requirements for claiming conformance with the DULO specification.

Only implementations satisfying all mandatory requirements defined by this specification MAY claim DULO compatibility.

---

## 14.2 Conformance Levels

Implementations SHALL declare one of the following levels:

Core

Extended

Full

The declared level SHALL accurately reflect implemented functionality.

---

## 14.3 Mandatory Behaviour

Mandatory requirements are identified using:

SHALL

MUST

SHALL NOT

MUST NOT

Failure to satisfy mandatory requirements constitutes non-conformance.

---

## 14.4 Optional Behaviour

Optional functionality is identified using:

MAY

SHOULD

OPTIONAL

Optional behaviour SHALL NOT alter mandatory semantics.

---

## 14.5 Observable Behaviour

Conformance SHALL be evaluated using externally observable behaviour.

Internal implementation details remain implementation-defined.

---

## 14.6 Compatibility

A conforming implementation SHALL:

Load conforming workbooks.

Preserve supported information.

Maintain deterministic calculations.

Handle unknown optional extensions safely.

---

## 14.7 Compliance Testing

Implementations SHOULD execute the official DULO Compliance Suite.

The suite SHALL include:

Workbook Tests

Formula Tests

Calculation Tests

Storage Tests

Extension Tests

Performance Tests

Security Tests

AI Interface Tests

---

## 14.8 Reference Behaviour

Where multiple implementation strategies exist,

observable behaviour SHALL remain identical.

---

## 14.9 Version Compatibility

Implementations SHALL declare supported specification versions.

Unsupported versions SHALL fail safely.

---

## 14.10 Reserved Features

Reserved features SHALL remain ignored unless explicitly implemented.

Reserved fields SHALL remain preserved.

---

## 14.11 Future Revisions

Future specification revisions SHALL preserve compatibility whenever practical.

Breaking changes SHOULD be minimized.

---

## 14.12 Compliance Declaration

Conforming implementations MAY publish:

Supported Specification Version

Conformance Level

Optional Features

Known Deviations

Extension Support

Compliance Date

---

## 14.13 Certification

Future DULO Certification programs MAY be established.

Certification SHALL remain independent of implementation language.

---

## 14.14 Conformance Checklist

✓ Mandatory requirements satisfied.

✓ Optional features documented.

✓ Compatibility declared.

✓ Specification version declared.

✓ Compliance testing completed.

✓ Observable behaviour conforms.

✓ Unknown features safely handled.

# Appendix A — DULO Standard Function Library

## A.1 Overview

This appendix defines the standard function library required by conforming DULO implementations.

All standard functions defined herein SHALL produce deterministic results given identical inputs unless explicitly designated as volatile.

Implementations MAY provide additional functions provided they do not alter the behavior of standard functions.

---

## A.2 Function Categories

The Standard Library consists of the following mandatory categories:

Arithmetic

Statistical

Logical

Text

Date & Time

Lookup

Information

Financial

Engineering

Database

Array

Matrix

Trigonometry

Conversion

Bitwise

Future categories MAY be introduced.

---

## A.3 Function Definition Format

Every standard function SHALL define:

Purpose

Syntax

Parameters

Return Type

Possible Errors

Examples

Edge Cases

Complexity Notes

Implementation Notes

Conformance Requirements

---

## A.4 Arithmetic Functions

Mandatory functions include:

SUM()

PRODUCT()

ABS()

POWER()

SQRT()

MOD()

ROUND()

ROUNDUP()

ROUNDDOWN()

CEILING()

FLOOR()

SIGN()

EXP()

LN()

LOG()

LOG10()

PI()

RAND()

RANDBETWEEN()

Future arithmetic functions MAY be introduced.

---

## A.5 Statistical Functions

Mandatory functions include:

AVERAGE()

MEDIAN()

MODE()

MIN()

MAX()

COUNT()

COUNTA()

COUNTBLANK()

COUNTIF()

COUNTIFS()

STDEV()

VAR()

PERCENTILE()

QUARTILE()

RANK()

Future statistical functions MAY be introduced.

---

## A.6 Logical Functions

Mandatory functions include:

IF()

IFS()

AND()

OR()

NOT()

XOR()

TRUE()

FALSE()

SWITCH()

CHOOSE()

---

## A.7 Text Functions

Mandatory functions include:

LEFT()

RIGHT()

MID()

LEN()

LOWER()

UPPER()

PROPER()

TRIM()

REPLACE()

SUBSTITUTE()

TEXT()

VALUE()

CONCAT()

TEXTJOIN()

SPLIT()

FIND()

SEARCH()

---

## A.8 Date & Time Functions

Mandatory functions include:

TODAY()

NOW()

YEAR()

MONTH()

DAY()

HOUR()

MINUTE()

SECOND()

DATE()

TIME()

DATEDIF()

NETWORKDAYS()

WORKDAY()

EOMONTH()

---

## A.9 Lookup Functions

Mandatory functions include:

LOOKUP()

VLOOKUP()

HLOOKUP()

XLOOKUP()

MATCH()

INDEX()

OFFSET()

FILTER()

SORT()

UNIQUE()

TAKE()

DROP()

---

## A.10 Information Functions

Mandatory functions include:

ISNUMBER()

ISTEXT()

ISBLANK()

ISERROR()

ISFORMULA()

TYPE()

ERROR.TYPE()

CELL()

INFO()

---

## A.11 Financial Functions

Mandatory functions include:

PMT()

PV()

FV()

RATE()

NPV()

IRR()

SLN()

DB()

DDB()

---

## A.12 Engineering Functions

Mandatory functions include:

BIN2DEC()

DEC2BIN()

HEX2DEC()

DEC2HEX()

BITAND()

BITOR()

BITXOR()

BITSHIFT()

---

## A.13 Array Functions

Mandatory functions include:

ARRAY()

MAP()

FILTER()

REDUCE()

SCAN()

MAKEARRAY()

EXPAND()

TOCOL()

TOROW()

WRAPROWS()

WRAPCOLS()

---

## A.14 Matrix Functions

Mandatory functions include:

MDETERM()

MINVERSE()

MMULT()

TRANSPOSE()

IDENTITY()

Future matrix functions MAY be introduced.

---

## A.15 Function Naming

Function names SHALL be case-insensitive.

Reserved names SHALL NOT be redefined.

---

## A.16 Volatile Functions

The following functions are volatile:

NOW()

TODAY()

RAND()

RANDBETWEEN()

Additional volatile functions SHALL explicitly declare volatility.

---

## A.17 Error Behaviour

Functions SHALL return deterministic errors.

Errors SHALL propagate according to Chapter 6.

---

## A.18 Future Functions

Unknown functions SHALL generate:

#NAME?

unless otherwise defined.

---

## A.19 Conformance

A conforming implementation SHALL implement every mandatory function defined by this appendix unless explicitly declared optional.

# Appendix B — Storage & File Format Reference

## B.1 Overview

This appendix defines the logical storage representation of DULO workbooks.

The storage reference is normative.

Physical implementations MAY differ internally provided observable behaviour remains conformant.

---

## B.2 Logical Workbook Structure

Workbook

├── Metadata

├── Worksheets

├── Shared Resources

├── Named Objects

├── Charts

├── Extensions

├── AI Metadata

├── Recovery Information

└── Reserved Fields

---

## B.3 Metadata Layout

Mandatory metadata includes:

Workbook Identifier

Specification Version

Creation Timestamp

Modification Timestamp

Locale

Calculation Mode

Workbook Settings

Reserved Fields

---

## B.4 Worksheet Layout

Each worksheet SHALL contain:

Worksheet Identifier

Worksheet Name

Dimensions

Cell Collection

Named Objects

Charts

Comments

Metadata

Extension Objects

---

## B.5 Cell Representation

Each cell SHALL define:

Address

Stored Value

Display Value

Formula

Formatting

Metadata

Dependencies

History (optional)

---

## B.6 Formula Storage

Stored formulas SHALL preserve:

Original Text

Parsed Representation (optional)

Compiled Representation (optional)

Dependency Metadata

Cache Metadata

---

## B.7 Style Storage

Shared styles SHALL contain:

Identifier

Font

Colour

Borders

Alignment

Number Format

Protection

Custom Properties

---

## B.8 Chart Storage

Charts SHALL preserve:

Identifier

Chart Type

Series

Axes

Legend

Formatting

Metadata

---

## B.9 Extension Storage

Extensions SHALL store:

Identifier

Version

Provider

Configuration

Permissions

Custom Data

---

## B.10 AI Metadata

Optional AI metadata MAY include:

Conversation References

Generated Formula History

Explanation Metadata

Prompt References

Provider Metadata

Implementations SHALL NOT require AI metadata.

---

## B.11 Serialization Order

Recommended logical serialization order:

Metadata

Styles

Named Objects

Worksheets

Charts

Extensions

AI Metadata

Recovery Information

Reserved Fields

---

## B.12 Recovery Records

Recovery information MAY include:

Journal

Transaction State

Snapshot Reference

Checksum

Recovery Timestamp

---

## B.13 Compatibility

Unknown objects SHALL remain preserved whenever possible.

---

## B.14 Reserved Regions

Reserved regions SHALL remain untouched by compliant implementations.

---

## B.15 Version Migration

Migration between specification versions SHOULD preserve all supported workbook information.

---

## B.16 Conformance

Storage implementations SHALL preserve every mandatory workbook object defined by this specification.

# Appendix C — DULO Conformance & Compliance Suite

## C.1 Overview

This appendix defines the official conformance and compliance procedures for DULO implementations.

A conforming implementation SHALL satisfy every mandatory requirement contained within the core specification and SHALL successfully execute the applicable compliance tests defined herein.

This appendix serves as the primary reference for validation, interoperability testing, regression testing, and certification.

---

## C.2 Objectives

The Compliance Suite SHALL verify:

Workbook Behaviour

Formula Evaluation

Calculation Determinism

Storage Integrity

Extension Compatibility

Security Behaviour

Performance Characteristics

Future Compatibility

---

## C.3 Compliance Levels

The following compliance levels are defined.

Core

Extended

Full

Each implementation SHALL declare its supported compliance level.

---

## C.4 Test Categories

The official Compliance Suite consists of:

Workbook Tests

Worksheet Tests

Cell Tests

Formula Tests

Calculation Tests

Storage Tests

Formatting Tests

Chart Tests

Extension Tests

AI Interface Tests

Performance Tests

Security Tests

Recovery Tests

Compatibility Tests

Regression Tests

Future versions MAY introduce additional categories.

---

## C.5 Workbook Tests

Workbook tests SHALL verify:

Workbook Creation

Workbook Saving

Workbook Loading

Workbook Metadata

Workbook Integrity

Workbook Recovery

Workbook Versioning

Workbook Migration

---

## C.6 Worksheet Tests

Worksheet tests SHALL verify:

Creation

Deletion

Renaming

Ordering

Visibility

Protection

Dimensions

Metadata

---

## C.7 Cell Tests

Cell tests SHALL verify:

Address Resolution

Value Storage

Formula Storage

Formatting

Metadata

Validation

History (if implemented)

Dependency Tracking

---

## C.8 Formula Tests

Formula tests SHALL verify:

Arithmetic

Logical

Statistical

Lookup

Date & Time

Engineering

Array Functions

Matrix Functions

Nested Expressions

Dynamic Arrays

Operator Precedence

Reference Behaviour

Error Propagation

Volatile Functions

Every standard function defined in Appendix A SHALL possess at least one reference test.

---

## C.9 Calculation Tests

Calculation tests SHALL verify:

Dependency Graph

Incremental Recalculation

Dirty Cell Tracking

Parallel Calculation

Lazy Evaluation

Circular References

Cache Invalidation

Transaction Behaviour

Recovery

Deterministic Results

---

## C.10 Storage Tests

Storage tests SHALL verify:

Serialization

Deserialization

Streaming

Compression

Snapshots

Checksums

Metadata Preservation

Reserved Fields

Future Compatibility

---

## C.11 Formatting Tests

Formatting tests SHALL verify:

Fonts

Colours

Borders

Alignment

Styles

Conditional Formatting

Validation Rules

Merged Cells

Hidden Objects

---

## C.12 Chart Tests

Chart tests SHALL verify:

Chart Creation

Series

Axes

Legends

Titles

Formatting

Dynamic Updates

Export

Metadata Preservation

---

## C.13 Extension Tests

Extension tests SHALL verify:

Plugin Loading

Plugin Removal

Version Compatibility

Permissions

Isolation

Failure Recovery

Custom Functions

SDK Behaviour

Language Bindings

---

## C.14 AI Interface Tests

AI interface tests SHALL verify:

Provider Registration

Session Management

Tool Invocation

Permission Enforcement

Workbook Safety

Context Isolation

Provider Independence

Failure Handling

---

## C.15 Security Tests

Security tests SHALL verify:

Workbook Validation

Permission Enforcement

Plugin Isolation

Formula Safety

Integrity Verification

Recovery

Logging

Deterministic Failures

---

## C.16 Performance Tests

Performance tests SHOULD measure:

Workbook Loading

Workbook Saving

Calculation Throughput

Memory Usage

Streaming

Large Workbook Behaviour

Cache Efficiency

Parallel Scaling

Performance benchmarks SHALL be documented.

---

## C.17 Compatibility Tests

Compatibility tests SHALL verify:

Older Workbook Versions

Reserved Fields

Unknown Extensions

Unknown Metadata

Unknown Objects

Future Specification Versions

---

## C.18 Regression Tests

Every released implementation SHOULD maintain a regression suite.

Regression suites SHOULD include:

Resolved Bugs

Edge Cases

Historical Behaviour

Performance Regressions

Security Regressions

---

## C.19 Reference Workbooks

The official DULO Compliance Suite SHALL include reference workbooks.

Recommended workbook categories include:

Basic Workbook

Large Workbook

Formula Workbook

Financial Workbook

Scientific Workbook

Engineering Workbook

Chart Workbook

Stress Workbook

Extension Workbook

Security Workbook

AI Workbook

Recovery Workbook

---

## C.20 Edge Cases

Reference tests SHOULD include:

Maximum Rows

Maximum Columns

Maximum Formula Length

Deep Dependency Chains

Large Dynamic Arrays

Large Charts

Large Named Ranges

Large Tables

Nested Functions

Circular References

Large Metadata

Large Extensions

---

## C.21 Expected Results

Every compliance workbook SHALL define:

Expected Output

Expected Errors

Expected Formatting

Expected Metadata

Expected Charts

Expected Calculation Results

Expected Storage Behaviour

---

## C.22 Failure Reporting

Compliance failures SHOULD report:

Test Identifier

Component

Expected Behaviour

Observed Behaviour

Severity

Specification Reference

Suggested Resolution

---

## C.23 Certification

Implementations successfully passing all mandatory tests MAY declare:

"DULO Conformant"

Certification SHALL specify:

Specification Version

Compliance Level

Supported Extensions

Optional Features

Certification Date

---

## C.24 Future Compliance

Future versions of the Compliance Suite SHALL preserve compatibility whenever practical.

Deprecated tests MAY remain available for historical validation.

---

## C.25 Final Conformance Checklist

✓ Workbook model conforms.

✓ Formula language conforms.

✓ Calculation engine conforms.

✓ Storage layer conforms.

✓ Spreadsheet features conform.

✓ Chart subsystem conforms.

✓ Extension system conforms.

✓ AI interface conforms.

✓ Performance requirements documented.

✓ Security requirements satisfied.

✓ Compatibility verified.

✓ Regression suite maintained.

✓ Official compliance suite completed.

---

End of DULO Specification.

Version 0.1 Draft

Licensed under the DULO PACK LICENSE (DPL)

© Avyaan Mishra (Doub.creator_00001). All rights reserved.