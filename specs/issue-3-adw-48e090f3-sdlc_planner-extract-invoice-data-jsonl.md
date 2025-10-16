# Feature: Extract Invoice Data to JSONL Format

## Metadata
issue_number: `3`
adw_id: `48e090f3`
issue_json: `{"number":3,"title":"Extract invoice data to JSONL format","body":"## Objective\nParse 5 sample PDF invoices and extract structured data matching the format in sample/invoices_parsed_final-ms2.xlsx. Output as JSONL (JSON Lines format) with one invoice per line.\n\n## Fields to Extract\nFrom each PDF invoice, extract:\n- Invoice Number\n- Bill To Company\n- Bill To Person  \n- Bill To Address\n- Ship To\n- Invoice Date\n- PO Number\n- FOB\n- Terms\n- Shipped Via\n- PPD/COLL\n- Unit Price\n- Total\n- Total Description\n- Subtotal\n- Total Due\n- Quantity\n- Description (item description)\n- Miscellaneous (bank/account information)\n\n## Sample Files\nLocated in `sample/` directory:\n- 13499 - DMS Enterprise Sri Lanka.pdf\n- 13499 - DMS Enterprise Sri Lanka-2.pdf\n- 13500 - GP Machine Tech Inc.pdf\n- 13501 - Agrasyst.pdf\n- 13530 - Bloomin Foods Ltd..pdf\n\n## Reference Output\n`sample/invoices_parsed_final-ms2.xlsx` contains the expected output format\n\n## Expected Deliverable\n- One JSONL file (`output/invoices.jsonl`) with all 5 invoices extracted\n- Each line is a JSON object with all 19 fields\n- Data matches the structure in the sample Excel file"}`

## Feature Description
This feature implements an automated invoice data extraction system that parses PDF invoices and extracts structured data into JSONL format. The system will analyze 5 sample PDF invoices from various companies, extract 19 specific fields from each invoice, and output the results in JSON Lines format (one JSON object per line). The extraction must match the structure and data accuracy shown in a reference Excel file (sample/invoices_parsed_final-ms2.xlsx).

The feature addresses the need to convert unstructured invoice PDF documents into machine-readable, structured data that can be easily processed, queried, and integrated into downstream systems such as accounting software, data warehouses, or analytics platforms.

## User Story
As a business analyst or data engineer
I want to automatically extract structured data from PDF invoices
So that I can analyze invoice data, populate databases, and automate accounting workflows without manual data entry

## Problem Statement
Invoice data is typically stored in PDF format, which is human-readable but not machine-readable. Manually extracting 19 fields from each invoice is time-consuming, error-prone, and does not scale. Organizations need an automated way to convert PDF invoices into structured data (JSONL) that can be:
- Imported into databases
- Analyzed using data analytics tools
- Integrated with accounting systems
- Processed by downstream automation workflows

The challenge is to accurately extract diverse invoice formats from different companies while maintaining data integrity and matching the reference output format.

## Solution Statement
We will implement a Python-based invoice extraction pipeline using PDF parsing libraries (pypdf2 or pdfplumber) and optionally AI-powered extraction (via OpenAI or Anthropic APIs) for intelligent field recognition. The solution includes:

1. **PDF Text Extraction**: Use pdfplumber to extract raw text and layout information from each PDF
2. **Field Extraction Logic**: Implement pattern matching and parsing rules to identify and extract 19 specific fields
3. **Data Validation**: Cross-reference extracted data with the reference Excel file structure
4. **JSONL Output**: Generate clean, formatted JSONL output with one invoice per line
5. **Testing & Validation**: Verify accuracy by comparing against reference data

The solution will be modular, allowing for future expansion to process additional invoices or integrate with web applications.

## Relevant Files
Use these files to implement the feature:

Since this is a greenfield project focused on invoice extraction, the following files are needed:

### New Files

- **`src/extract_invoices.py`** - Main extraction script that orchestrates the PDF parsing and JSONL generation process
- **`src/parsers/pdf_parser.py`** - PDF parsing module using pdfplumber to extract text and layout information from invoices
- **`src/parsers/field_extractor.py`** - Field extraction logic that identifies and extracts the 19 required fields using regex patterns and text parsing
- **`src/models/invoice.py`** - Data model (Pydantic) defining the Invoice schema with all 19 fields
- **`src/utils/validation.py`** - Validation utilities to verify extracted data quality and completeness
- **`src/utils/jsonl_writer.py`** - JSONL file writing utilities
- **`tests/test_pdf_parser.py`** - Unit tests for PDF parsing functionality
- **`tests/test_field_extractor.py`** - Unit tests for field extraction logic
- **`tests/test_invoice_model.py`** - Unit tests for Invoice data model validation
- **`tests/test_integration.py`** - Integration test that processes all 5 sample PDFs and validates output
- **`pyproject.toml`** - Python project configuration with dependencies (uv-managed)
- **`README.md`** - Project documentation explaining how to run the extraction script
- **`sample/`** - Directory for input PDF files (to be created, but PDFs should already exist per the issue)
- **`output/`** - Directory for output JSONL file
- **`reference/invoices_parsed_final-ms2.xlsx`** - Reference Excel file (should already exist in sample/)

### Existing Files to Reference

- **`.claude/commands/test_e2e.md`** - To understand E2E testing patterns (not directly applicable since this is a CLI tool, but useful for understanding validation approaches)
- **`.claude/commands/conditional_docs.md`** - Reviewed to check if additional documentation is needed (none matched)

## Implementation Plan

### Phase 1: Foundation
Set up the project structure and core dependencies:
1. Initialize Python project with uv package manager
2. Install required dependencies: pdfplumber, pydantic, openpyxl (for reading reference Excel), pytest
3. Create directory structure: src/, src/parsers/, src/models/, src/utils/, tests/, sample/, output/
4. Define Invoice data model with Pydantic (19 fields with proper types)
5. Read and analyze reference Excel file to understand expected output format

### Phase 2: Core Implementation
Build the PDF extraction and field parsing logic:
1. Implement PDF text extraction using pdfplumber
2. Implement field extraction logic with pattern matching for each of the 19 fields
3. Handle variations in invoice formats across different companies
4. Implement JSONL writer utility
5. Build main extraction script that processes all PDFs and generates output/invoices.jsonl

### Phase 3: Integration
Test, validate, and document the complete system:
1. Create comprehensive unit tests for each module
2. Create integration test that validates all 5 PDFs are processed correctly
3. Compare output against reference Excel to ensure accuracy
4. Add error handling and logging
5. Document usage in README.md

## Step by Step Tasks
IMPORTANT: Execute every step in order, top to bottom.

### 1. Project Initialization and Setup
- Create project directory structure: `src/`, `src/parsers/`, `src/models/`, `src/utils/`, `tests/`, `output/`
- Initialize Python project with uv: `uv init` or create `pyproject.toml`
- Add dependencies: `uv add pdfplumber pydantic openpyxl pytest`
- Verify sample PDFs exist in `sample/` directory
- Verify reference Excel file exists at `sample/invoices_parsed_final-ms2.xlsx`

### 2. Read and Analyze Reference Data
- Read `sample/invoices_parsed_final-ms2.xlsx` using openpyxl
- Document the exact schema: field names, data types, and example values
- Understand the expected output format for JSONL

### 3. Create Invoice Data Model
- Create `src/models/__init__.py`
- Create `src/models/invoice.py` with Pydantic model defining all 19 fields:
  - invoice_number (str)
  - bill_to_company (str)
  - bill_to_person (str)
  - bill_to_address (str)
  - ship_to (str)
  - invoice_date (str)
  - po_number (str)
  - fob (str)
  - terms (str)
  - shipped_via (str)
  - ppd_coll (str)
  - unit_price (str or float)
  - total (str or float)
  - total_description (str)
  - subtotal (str or float)
  - total_due (str or float)
  - quantity (str or int)
  - description (str)
  - miscellaneous (str)
- Add validation logic to ensure required fields are present

### 4. Implement PDF Text Extraction
- Create `src/parsers/__init__.py`
- Create `src/parsers/pdf_parser.py`
- Implement function `extract_text_from_pdf(pdf_path: str) -> str` using pdfplumber
- Extract both text content and layout information (useful for identifying field positions)
- Add error handling for corrupted or unreadable PDFs
- Create unit test `tests/test_pdf_parser.py` to verify text extraction works on at least one sample PDF

### 5. Implement Field Extraction Logic
- Create `src/parsers/field_extractor.py`
- Implement function `extract_fields(text: str) -> dict` that parses raw text and extracts all 19 fields
- Use regex patterns and text parsing to identify:
  - Invoice Number (typically near "Invoice #" or "Invoice No")
  - Bill To information (company, person, address)
  - Ship To information
  - Invoice Date
  - PO Number
  - Other metadata fields (FOB, Terms, Shipped Via, etc.)
  - Line item details (Quantity, Description, Unit Price, Total)
  - Summary totals (Subtotal, Total Due)
  - Miscellaneous (bank/account info, typically at bottom)
- Handle missing fields gracefully (use None or empty string)
- Create unit test `tests/test_field_extractor.py` to verify extraction logic

### 6. Implement JSONL Writer
- Create `src/utils/__init__.py`
- Create `src/utils/jsonl_writer.py`
- Implement function `write_jsonl(invoices: List[Invoice], output_path: str)` that writes Invoice objects to JSONL format
- Ensure each line is a valid JSON object (no trailing commas, proper escaping)
- Test JSONL output can be read back correctly

### 7. Implement Validation Utilities
- Create `src/utils/validation.py`
- Implement function `validate_invoice(invoice: Invoice) -> bool` to check data quality
- Implement function `compare_with_reference(extracted_invoices: List[Invoice], reference_path: str) -> dict` to compare extracted data with reference Excel
- Report differences and accuracy metrics

### 8. Build Main Extraction Script
- Create `src/extract_invoices.py`
- Implement main function that:
  1. Discovers all PDF files in `sample/` directory
  2. Processes each PDF using `pdf_parser.extract_text_from_pdf()`
  3. Extracts fields using `field_extractor.extract_fields()`
  4. Validates using `Invoice` model
  5. Collects all invoices into a list
  6. Writes to `output/invoices.jsonl` using `jsonl_writer.write_jsonl()`
- Add command-line interface (optional: use argparse) to specify input/output paths
- Add logging to track progress and errors

### 9. Create Unit Tests for Invoice Model
- Create `tests/test_invoice_model.py`
- Test Invoice model validation (required fields, type checking)
- Test serialization to JSON format
- Test handling of optional/missing fields

### 10. Create Integration Test
- Create `tests/test_integration.py`
- Test end-to-end flow: PDF → extraction → JSONL output
- Verify all 5 sample PDFs are processed successfully
- Verify output JSONL contains 5 lines (one per invoice)
- Validate each invoice has all 19 fields
- Compare extracted data against reference Excel file
- Ensure accuracy is acceptable (define threshold, e.g., 95% field accuracy)

### 11. Manual Validation Against Reference Data
- Run extraction script: `uv run python src/extract_invoices.py`
- Open `output/invoices.jsonl` and verify format
- Open `sample/invoices_parsed_final-ms2.xlsx` and manually compare a few invoices
- Document any discrepancies and iterate on field extraction logic

### 12. Add Error Handling and Logging
- Add try-catch blocks in main script for graceful error handling
- Add logging statements to track which PDF is being processed
- Log warnings for missing or low-confidence fields
- Create log file in `output/extraction.log`

### 13. Write README Documentation
- Create `README.md` in project root
- Document:
  - Project purpose and overview
  - Installation instructions (`uv sync`)
  - Usage instructions (`uv run python src/extract_invoices.py`)
  - Input requirements (PDF files in `sample/`)
  - Output format (JSONL in `output/`)
  - Field descriptions (19 fields)
  - Testing instructions (`uv run pytest`)
- Include example JSONL output snippet

### 14. Final Validation
- Execute all validation commands listed in the `Validation Commands` section below
- Ensure zero errors in tests
- Verify output matches reference data structure
- Confirm all 5 invoices are extracted successfully

## Testing Strategy

### Unit Tests

1. **PDF Parser Tests** (`tests/test_pdf_parser.py`)
   - Test text extraction from a valid PDF
   - Test handling of corrupted/missing PDF files
   - Test extraction of multi-page PDFs
   - Verify extracted text is non-empty and contains expected keywords

2. **Field Extractor Tests** (`tests/test_field_extractor.py`)
   - Test extraction of each individual field with mock text input
   - Test handling of missing fields
   - Test regex patterns against known invoice text samples
   - Test extraction from multiple invoice formats

3. **Invoice Model Tests** (`tests/test_invoice_model.py`)
   - Test Pydantic validation for required fields
   - Test JSON serialization/deserialization
   - Test handling of None/empty values for optional fields
   - Test data type enforcement (e.g., numbers vs strings)

4. **JSONL Writer Tests** (`tests/test_jsonl_writer.py`)
   - Test writing single invoice to JSONL
   - Test writing multiple invoices
   - Test reading back written JSONL to verify format
   - Test handling of special characters and escaping

5. **Validation Tests** (`tests/test_validation.py`)
   - Test invoice validation logic
   - Test comparison with reference data
   - Test accuracy metrics calculation

6. **Integration Tests** (`tests/test_integration.py`)
   - Test full pipeline: all 5 PDFs → JSONL output
   - Verify output file exists and has correct number of lines
   - Verify each line is valid JSON
   - Compare against reference Excel file

### Edge Cases

- **Missing Fields**: Some invoices may not have all 19 fields (e.g., no PO Number) - handle gracefully with None or empty string
- **Varied Formats**: Each company's invoice has a different layout - extraction logic must be flexible
- **Multi-page Invoices**: Some invoices span multiple pages - ensure all pages are processed
- **Special Characters**: Invoices may contain Unicode, currency symbols, newlines in addresses - ensure proper escaping in JSON
- **Numeric Formats**: Prices may have commas, dollar signs, or different decimal formats - normalize or preserve as strings
- **Empty/Null Values**: Handle cases where a field is present but empty
- **Duplicate Invoices**: The issue mentions "13499 - DMS Enterprise Sri Lanka.pdf" and "13499 - DMS Enterprise Sri Lanka-2.pdf" - verify these are different invoices or handle duplicates
- **OCR Errors**: If PDFs are scanned images, text extraction may be inaccurate - consider using OCR libraries if needed (pytesseract) or mention limitations
- **File Access Errors**: Handle cases where PDF files are locked, corrupted, or missing

## Acceptance Criteria

1. **Output File Created**: `output/invoices.jsonl` file is created and contains exactly 5 lines (one per invoice)
2. **Valid JSONL Format**: Each line in the output file is a valid JSON object that can be parsed
3. **All Fields Present**: Each JSON object contains all 19 fields as specified in the issue
4. **Data Accuracy**: Extracted data matches the reference Excel file (`sample/invoices_parsed_final-ms2.xlsx`) with at least 95% accuracy across all fields
5. **Field Mapping Correct**: Field names in JSON output match the expected schema (invoice_number, bill_to_company, etc.)
6. **All PDFs Processed**: All 5 sample PDFs are successfully processed without errors:
   - 13499 - DMS Enterprise Sri Lanka.pdf
   - 13499 - DMS Enterprise Sri Lanka-2.pdf
   - 13500 - GP Machine Tech Inc.pdf
   - 13501 - Agrasyst.pdf
   - 13530 - Bloomin Foods Ltd..pdf
7. **Tests Pass**: All unit tests and integration tests pass with zero failures
8. **Repeatable**: Running the extraction script multiple times produces identical output (deterministic)
9. **Error Handling**: Script handles missing files, corrupted PDFs, and other errors gracefully without crashing
10. **Documentation**: README.md clearly explains how to install dependencies, run the script, and interpret the output

## Validation Commands

Execute every command to validate the feature works correctly with zero regressions.

```bash
# 1. Verify project structure is set up correctly
ls -la src/ src/parsers/ src/models/ src/utils/ tests/ sample/ output/

# 2. Verify all sample PDFs exist
ls -la sample/*.pdf

# 3. Verify reference Excel file exists
ls -la sample/invoices_parsed_final-ms2.xlsx

# 4. Install dependencies
uv sync

# 5. Run all unit tests
uv run pytest tests/ -v

# 6. Run the extraction script
uv run python src/extract_invoices.py

# 7. Verify output file was created
ls -la output/invoices.jsonl

# 8. Verify output has exactly 5 lines (one per invoice)
wc -l output/invoices.jsonl

# 9. Verify each line is valid JSON
uv run python -c "import json; [json.loads(line) for line in open('output/invoices.jsonl')]"

# 10. Display first invoice (for manual inspection)
head -1 output/invoices.jsonl | uv run python -m json.tool

# 11. Verify all 19 fields are present in each invoice
uv run python -c "import json; lines = open('output/invoices.jsonl').readlines(); print('Fields in each invoice:'); [print(f'Invoice {i+1}: {len(json.loads(line))} fields') for i, line in enumerate(lines)]"

# 12. Run integration test that compares against reference Excel
uv run pytest tests/test_integration.py -v -s

# 13. Check for any errors in extraction log
cat output/extraction.log
```

## Notes

### Technology Choices

- **PDF Parsing**: Using `pdfplumber` instead of `pypdf2` because it provides better text extraction with layout information, which helps identify field positions
- **Data Validation**: Using `pydantic` for data modeling provides automatic validation, type checking, and JSON serialization
- **Reference Comparison**: Using `openpyxl` to read the reference Excel file for validation
- **Package Management**: Using `uv` as specified in the ADW framework for fast, reliable dependency management

### Potential Enhancements (Future)

- **AI-Powered Extraction**: Consider using OpenAI GPT-4 Vision or Anthropic Claude with vision capabilities for more intelligent field extraction, especially for complex or varied invoice formats
- **OCR Support**: If invoices are scanned images rather than text-based PDFs, integrate `pytesseract` or a cloud OCR service
- **Web Interface**: Build a simple Flask or FastAPI web interface for uploading PDFs and downloading JSONL
- **Database Integration**: Store extracted invoices in a PostgreSQL or SQLite database instead of just JSONL files
- **Batch Processing**: Support processing thousands of invoices from cloud storage (S3, GCS)
- **Confidence Scores**: Add confidence scores for each extracted field to help identify low-quality extractions
- **Invoice Templates**: Create template-based extraction where users can define extraction rules for specific invoice formats

### Known Limitations

- The extraction accuracy depends heavily on the consistency of invoice formats
- Hand-written or heavily formatted invoices may require OCR or AI-based extraction
- The solution assumes PDFs contain extractable text (not scanned images)
- Field extraction rules are tailored to the 5 sample invoices and may need refinement for new invoice formats

### Dependencies to Add

```toml
[project]
dependencies = [
    "pdfplumber>=0.11.0",
    "pydantic>=2.0.0",
    "openpyxl>=3.1.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=7.4.0",
    "pytest-cov>=4.1.0",
]
```

### Development Workflow

1. Start by examining one sample PDF manually to understand structure
2. Implement extraction logic incrementally (field by field)
3. Test against reference data after each field implementation
4. Iterate on regex patterns and parsing logic
5. Once one invoice works well, apply to remaining 4 invoices
6. Refine extraction logic to handle variations across all 5 invoices
