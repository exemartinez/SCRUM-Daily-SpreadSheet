# SCRUM Daily Spreadsheet

This project is a Pharo/Smalltalk experiment for consolidating daily operational data into a spreadsheet-friendly format.

The original motivation was simple: take the different files produced around the DailyBot workflow, read them in Pharo, and move toward a single consolidated spreadsheet that could later be pushed to Google Sheets or handled locally as Excel data.

## Why this exists

At the time I started this project, I needed a lightweight way to:

- model spreadsheet-like structures inside a Pharo image,
- open and inspect Excel workbooks,
- prepare a path toward consolidating DailyBot exports,
- eventually connect that flow with Google Spreadsheet APIs.

The repository captures that first working exploration and the shape of the solution I wanted to build.

## What is in the repository

- `GoogleDrive-Analyzer.st`
  The project-specific Smalltalk source. It currently defines:
  - `GoogleAPIConnector`, a thin placeholder wrapper for the Google Sheets endpoint,
  - `SpreadSheet`, an abstraction intended to behave like a spreadsheet handler inside the image.

- `GoogleDrive-Analyzer-Tests.st`
  A set of tests that document the intended direction of the project:
  - spreadsheet open/read/write behavior,
  - consolidation of multiple spreadsheet formats,
  - support for Daily Questions / analysts / trading-room variants,
  - basic web-service availability checks.

- `Tabular.st`
  A vendored export of the `Tabular` library used to parse spreadsheet files, especially `.xlsx` workbooks.

## Current state

This project should be read as an early-stage prototype, not a finished product.

What already exists:

- a Smalltalk package export that can be filed into a Pharo image,
- a working dependency (`Tabular`) for reading spreadsheet content,
- an exploratory `SpreadSheet>>openFile:` path using `XLSXImporter`,
- tests that describe the intended domain model and workflow.

What is still incomplete:

- most spreadsheet mutation APIs (`row:col:value:`, tab creation, tab lookup),
- the real Google Sheets integration and authentication flow,
- a finished consolidator object,
- executable tests for most business behavior.

Several tests intentionally still assert `false`; they act more like a development checklist than a stable test suite.

## Expected environment

This code was authored for a Pharo image and stored as `.st` file-outs instead of a Monticello package.

You will need a Pharo environment with the libraries typically used by this code available in the image, including support for:

- Zinc (`ZnClient`, `ZnServer`),
- ZIP/archive handling,
- the classes loaded from `Tabular.st`.

## Loading the project

The simplest workflow is to file the sources into a Pharo image:

1. Open Pharo.
2. File in `Tabular.st`.
3. File in `GoogleDrive-Analyzer.st`.
4. File in `GoogleDrive-Analyzer-Tests.st` if you want the test classes as well.

I kept the sources as plain file-outs because this repository was meant to be portable and easy to import into another image without relying on Monticello packaging.

## Usage direction

The current implementation is centered on reading spreadsheet files and exploring their contents.

Example intent:

```smalltalk
sheet := SpreadSheet new.
sheet openFile: '/path/to/workbook.xlsx'.
```

Under the hood, the prototype relies on `XLSXImporter` from `Tabular` to open the workbook and inspect worksheets/cells.

The Google API connector is only a stub at this stage. Its role is to define the place where Google Spreadsheet connectivity should eventually live, with `https://spreadsheets.google.com/feeds` as the default endpoint.

## Tests

The tests are useful mainly as design notes.

They describe the system I wanted to end up with:

- a generic spreadsheet abstraction,
- specialized spreadsheet formats for different DailyBot outputs,
- a consolidator capable of merging compatible sheets,
- connectivity checks for local web services and Google endpoints.

Do not expect the test suite to pass in its current state without additional implementation work and environment setup.

## Project intent

This repository is best understood as the seed of a larger tool: a Pharo-based spreadsheet consolidation layer for SCRUM/daily reporting workflows, with Google Sheets integration as the eventual destination.

If I were continuing this project, the next steps would be:

- finish the `SpreadSheet` API around tabs, rows, and cell access,
- introduce explicit domain models for each DailyBot export format,
- implement a real consolidator object,
- isolate infrastructure concerns from spreadsheet behavior,
- replace placeholder tests with executable examples and fixtures.

## Notes

- The repository name references SCRUM daily spreadsheet work, while the source package name is `GoogleDrive-Analyzer`; both point to the same idea.
- `Tabular.st` is large because it contains the bundled spreadsheet parsing dependency as a full Smalltalk file-out.
