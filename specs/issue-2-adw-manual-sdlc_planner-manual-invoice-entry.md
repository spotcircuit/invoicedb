# Feature: Manual Invoice Entry System

## Metadata
issue_number: `2`
adw_id: `manual`
issue_json: `{"body":"## Overview\nBuild a normalized relational database system to process 3800 invoices from the sample folder, enabling business intelligence and analytics on customer ordering patterns.\n\n## Problem Statement\nWe have ~3800 invoices in various formats (PDF, Word docs) that need to be:\n- Extracted and parsed automatically\n- Normalized into a relational database structure\n- Deduplicated (some invoices may be duplicates)\n- Made available for business analytics\n\n## Requirements\n\n### Data Extraction\n- [ ] Read and parse PDF invoices (text-based and image-based with OCR)\n- [ ] Read and parse Word document invoices\n- [ ] Handle messy/inconsistent data formats\n- [ ] No duplicate records in final database\n\n### Database Design\n- [ ] Normalized relational schema (3NF or higher)\n- [ ] Identify all data items from sample invoices\n- [ ] Entities: Customers, Invoices, Line Items, Products/Services, Addresses\n- [ ] Support for querying customer patterns, ordering frequency, amounts\n\n### Data Quality\n- [ ] Data normalization (standardize customer names, addresses, etc.)\n- [ ] Validation rules for data quality\n- [ ] Handle missing/incomplete data gracefully\n- [ ] Deduplication algorithm (content-based hashing + fuzzy matching)\n\n### Export & Analytics\n- [ ] Export to CSV/Excel spreadsheet\n- [ ] Business intelligence queries:\n  - Customer ordering patterns\n  - Order frequency per customer\n  - Revenue analysis\n  - Product/service popularity\n  - Trending analysis\n\n### Iterative Processing\n- [ ] Batch processing framework (process in batches of 100-200)\n- [ ] Progress tracking and resumption capability\n- [ ] Error handling and retry logic\n- [ ] Quality metrics after each batch\n\n## Technical Approach\n\n### Technology Stack\n- **Database**: PostgreSQL with SQLAlchemy ORM\n- **Migrations**: Alembic\n- **PDF Parsing**: pdfplumber, PyPDF2\n- **Word Parsing**: python-docx\n- **OCR**: pytesseract (for image-based PDFs)\n- **Deduplication**: hashlib, fuzzywuzzy\n- **API**: FastAPI\n- **Export**: pandas, openpyxl\n\n### Agent Expert Integration\n- Create `invoice_parsing` Agent Expert (self-improving)\n- Expert learns from each batch and improves extraction accuracy\n- Institutional knowledge capture for future invoice processing\n\n## Acceptance Criteria\n- [ ] All 3800 invoices processed successfully (>95% accuracy)\n- [ ] Duplicates identified and removed\n- [ ] Normalized database schema implemented\n- [ ] Data quality report generated\n- [ ] Export CSV/Excel functional\n- [ ] Analytics queries validated\n- [ ] Processing time <30 minutes per 100 invoices\n- [ ] Agent Expert improves parsing accuracy over batches\n\n## Sample Data\n- 5 sample invoices available for initial testing\n- Sample CSV available showing expected output format (open to improvements)\n- All invoices vary in format and structure\n\n## Success Metrics\n- Parsing accuracy: 95%+\n- Deduplication accuracy: 98%+\n- Data quality score: 95%+\n- Processing efficiency: full batch in <4 hours\n- Agent Expert learning curve: accuracy improves by 10%+ from first to last batch","number":2,"title":"Build normalized invoice database with PDF/Word extraction and deduplication"}`

## Feature Description
This feature adds a **manual invoice entry interface** to complement the automated PDF/Word extraction system. While the main system processes invoices automatically, this feature enables users to manually enter invoice data for cases where:
- Automated extraction fails or produces inaccurate results
- Invoices are in formats not supported by the parser (handwritten, faxed, unusual formats)
- Users need to create test data or correct extracted data
- Quick entry of single invoices is needed without waiting for batch processing

The manual entry system provides a user-friendly web form with validation, duplicate checking, and immediate database insertion, integrated seamlessly with the existing invoice management application.

## User Story
As a data entry specialist working with the invoice management system
I want to manually enter invoice data through a web form when automated extraction fails or for special cases
So that I can ensure all invoices are recorded in the database regardless of their format or quality, and correct any extraction errors

## Problem Statement
The automated invoice processing system (Issue #2) handles bulk extraction from PDF and Word documents, but several scenarios require manual data entry:
1. **Extraction Failures**: Some PDFs are poorly formatted, image-only without OCR capability, or corrupted, resulting in failed or inaccurate automated extraction
2. **Unsupported Formats**: Handwritten invoices, faxed copies, or unusual document formats cannot be processed automatically
3. **Data Correction**: When automated extraction produces incorrect data (wrong amounts, misread vendor names, incorrect dates), users need to manually correct these entries
4. **Edge Cases**: Paper invoices that need to be entered before scanning, or invoices received via phone/email that need immediate entry
5. **Testing and Development**: Need to create sample data for testing without processing actual PDF files

Without a manual entry interface, these edge cases either result in missing data or require direct database manipulation, which is error-prone and not user-friendly.

## Solution Statement
Implement a **Manual Invoice Entry Form** that provides:
1. **Web-Based Form Interface**: A clean, intuitive form with fields for all invoice data (invoice number, customer name, address, date, line items, amounts, etc.)
2. **Real-Time Validation**: Client-side and server-side validation to ensure data quality (required fields, date formats, numeric amounts, valid addresses)
3. **Duplicate Detection**: Check for existing invoices with same invoice number before saving, with option to review/update existing records
4. **Line Items Management**: Dynamic form to add/remove multiple line items (products/services) within a single invoice
5. **Customer Autocomplete**: Suggest existing customers as user types to maintain data consistency and reduce duplicates
6. **Immediate Feedback**: Save directly to database with confirmation, show validation errors clearly, display success message with link to view the created invoice
7. **Integration with Existing System**: Use same database schema, CRUD operations, and validation rules as automated extraction system

The solution provides a safety net for the automated system while maintaining data quality and consistency across both manual and automated entry methods.

## Relevant Files
Use these files to implement the feature:

Based on issue #1 plan and the existing agentic framework, the following files are relevant:

### Existing Files to Read/Modify

#### Backend Files
- `app/server/main.py` - Add POST endpoint for manual invoice creation
- `app/server/models.py` - Review existing Invoice, Customer, LineItem, Address models
- `app/server/schemas.py` - Add Pydantic schemas for manual invoice creation request
- `app/server/crud.py` - Add manual invoice creation function with validation
- `app/server/database.py` - Review database session management
- `app/server/utils.py` - Add validation utility functions

#### Frontend Files
- `app/client/src/App.tsx` - Add route for manual invoice entry page
- `app/client/src/api/client.ts` - Add API function for creating manual invoice
- `app/client/src/types/invoice.ts` - Review/extend Invoice TypeScript interfaces

#### Documentation Files
- `README.md` - Should exist from issue #1, read to understand project structure
- `.claude/commands/test_e2e.md` - Read to understand E2E test framework
- `.claude/commands/e2e/test_basic_query.md` - Read as example of E2E test structure
- `.claude/commands/conditional_docs.md` - Already read, relevant for understanding when to read docs

### New Files

#### Backend Files
- `app/server/validators.py` - Validation functions for manual invoice data (phone numbers, addresses, dates, amounts)
- `app/server/test_manual_invoice.py` - Tests for manual invoice entry endpoints and validation

#### Frontend Files
- `app/client/src/components/ManualInvoiceEntry.tsx` - Main manual invoice entry form component
- `app/client/src/components/LineItemForm.tsx` - Subcomponent for managing line items (add/remove rows)
- `app/client/src/components/CustomerAutocomplete.tsx` - Customer name autocomplete component
- `app/client/src/components/AddressForm.tsx` - Address input subcomponent with validation
- `app/client/src/hooks/useInvoiceForm.ts` - Custom React hook for form state management
- `app/client/src/hooks/useCustomerSearch.ts` - Custom React hook for customer autocomplete
- `app/client/src/utils/validation.ts` - Client-side validation functions

#### E2E Test Files
- `.claude/commands/e2e/test_manual_invoice_entry.md` - E2E test for creating a complete invoice manually
- `.claude/commands/e2e/test_duplicate_detection.md` - E2E test for duplicate invoice number detection
- `.claude/commands/e2e/test_customer_autocomplete.md` - E2E test for customer name autocomplete functionality

## Implementation Plan

### Phase 1: Foundation
1. **Review Existing Schema**: Read existing database models (Invoice, Customer, LineItem, Address, Product) from issue #1 implementation to understand normalized structure (3NF+)
2. **Design Validation Rules**: Define comprehensive validation rules for manual entry (required fields, data formats, business rules like invoice date not in future)
3. **Create Validation Utilities**: Implement server-side and client-side validation functions for dates, amounts, addresses, phone numbers, email addresses
4. **Plan Form Structure**: Design form layout with sections (customer info, invoice details, line items, totals) and field requirements

### Phase 2: Core Implementation
1. **Backend API Endpoint**: Create POST /api/invoices/manual endpoint that accepts complete invoice data (customer, invoice, line items, addresses) in single request
2. **Validation Layer**: Implement comprehensive server-side validation before database insertion (check required fields, validate formats, check for duplicates, enforce business rules)
3. **Customer Lookup API**: Create GET /api/customers/search?q={query} endpoint for autocomplete functionality
4. **Duplicate Detection**: Implement duplicate checking logic (query existing invoices by invoice_number, option to update existing or create new with different number)
5. **Manual Invoice Form Component**: Build main React form with sections for customer info, invoice metadata, and line items
6. **Line Items Manager**: Create dynamic line items component (add row, remove row, calculate line totals, update invoice total)
7. **Customer Autocomplete**: Implement typeahead search for customer names with debouncing and keyboard navigation
8. **Form State Management**: Use React hooks (useState, useReducer) to manage complex form state including nested line items

### Phase 3: Integration
1. **API Integration**: Connect frontend form to backend API with proper error handling and loading states
2. **Real-Time Validation**: Implement client-side validation with immediate feedback (show errors on blur, highlight invalid fields)
3. **Success Flow**: After successful save, show confirmation message and provide link to view the created invoice in the main invoice list
4. **Error Handling**: Display user-friendly error messages for validation failures, duplicate detection, and server errors
5. **Navigation Integration**: Add "Manual Entry" button/link to main invoice list page, add breadcrumb navigation
6. **Duplicate Resolution UI**: When duplicate detected, show dialog with options (update existing, create with new number, cancel)
7. **Form Reset**: Provide "Clear Form" button and "Save & Create Another" button for efficient data entry

## Step by Step Tasks
IMPORTANT: Execute every step in order, top to bottom.

### 1. Review Existing Database Schema and Models
- Read `app/server/models.py` to understand Invoice, Customer, LineItem, Address, Product models and relationships
- Document required fields, optional fields, and relationships between entities
- Identify validation constraints already defined in models (unique constraints, foreign keys, NOT NULL)
- Review normalization structure to understand how manual entry must maintain data consistency

### 2. Create Validation Utility Module
- Create `app/server/validators.py` with validation functions:
  - `validate_invoice_number(invoice_number: str) -> bool` - Check format and uniqueness
  - `validate_date(date_str: str) -> bool` - Parse and validate date format, check not in future
  - `validate_amount(amount: str) -> float` - Parse currency amount, check positive
  - `validate_email(email: str) -> bool` - Validate email format
  - `validate_phone(phone: str) -> bool` - Validate phone number format
  - `validate_address(address: dict) -> bool` - Validate address components
  - `check_duplicate_invoice(invoice_number: str, db: Session) -> Optional[Invoice]` - Check for existing invoice
- Create `app/client/src/utils/validation.ts` with equivalent client-side validation functions
- Write unit tests in `app/server/test_validators.py`

### 3. Implement Customer Search API Endpoint
- Add GET /api/customers/search endpoint to `app/server/main.py`
- Implement `search_customers(query: str, limit: int, db: Session)` in `app/server/crud.py`
- Use ILIKE SQL query for fuzzy customer name matching (e.g., `WHERE name ILIKE '%{query}%'`)
- Return list of customers with id, name, address for autocomplete
- Add endpoint test in `app/server/test_main.py`

### 4. Implement Manual Invoice Creation API Endpoint
- Add Pydantic schemas to `app/server/schemas.py`:
  - `LineItemCreate` - product/service, quantity, unit_price, total
  - `ManualInvoiceCreate` - invoice_number, customer_id or customer_name, date, line_items, notes
  - `ManualInvoiceResponse` - complete invoice data with generated IDs
- Add POST /api/invoices/manual endpoint to `app/server/main.py`
- Implement `create_manual_invoice(invoice_data: ManualInvoiceCreate, db: Session)` in `app/server/crud.py`:
  - Validate all input data using validators module
  - Check for duplicate invoice_number
  - If customer_id not provided, look up or create customer by name
  - Create Invoice record
  - Create related LineItem records
  - Calculate and store totals
  - Return created invoice with all IDs
- Implement proper transaction handling (rollback on error)
- Add comprehensive error responses with field-level validation errors
- Write tests in `app/server/test_manual_invoice.py`

### 5. Create Frontend Form State Management Hook
- Create `app/client/src/hooks/useInvoiceForm.ts`:
  - Manage form state for customer info, invoice metadata, line items array
  - Provide functions to add/remove line items
  - Calculate line item totals and invoice total automatically
  - Manage validation errors state
  - Provide reset function to clear form
- Create `app/client/src/hooks/useCustomerSearch.ts`:
  - Manage customer search query and results
  - Implement debounced search (300ms delay)
  - Handle keyboard navigation (arrow up/down, enter to select)
  - Provide select customer function

### 6. Build Line Item Form Component
- Create `app/client/src/components/LineItemForm.tsx`:
  - Render table with columns: Product/Service, Quantity, Unit Price, Total, Actions
  - Each row is an input form for one line item
  - Implement Add Row button (adds empty line item to state)
  - Implement Remove Row button (removes line item from state)
  - Auto-calculate line total when quantity or unit_price changes (quantity * unit_price)
  - Display validation errors for each field
  - Display invoice subtotal at bottom (sum of all line totals)

### 7. Build Customer Autocomplete Component
- Create `app/client/src/components/CustomerAutocomplete.tsx`:
  - Text input that triggers search as user types
  - Display dropdown with matching customers below input
  - Highlight selected customer on keyboard navigation
  - On customer select, populate customer fields (name, address, phone, email)
  - Allow manual entry if customer not found
  - Show "Create New Customer" option if no matches

### 8. Build Address Form Component
- Create `app/client/src/components/AddressForm.tsx`:
  - Input fields for street, city, state/province, postal code, country
  - Client-side validation for required fields
  - Format validation for postal code based on country
  - Display validation errors inline
  - Reusable component for both billing and shipping addresses

### 9. Build Main Manual Invoice Entry Form
- Create `app/client/src/components/ManualInvoiceEntry.tsx`:
  - Form sections:
    1. Invoice Information (invoice_number, date, reference)
    2. Customer Information (CustomerAutocomplete + manual fields)
    3. Billing Address (AddressForm component)
    4. Line Items (LineItemForm component)
    5. Totals Display (subtotal, tax if applicable, total)
    6. Notes/Comments (textarea)
  - Form validation on submit (check all required fields, validate formats)
  - Show loading spinner during API call
  - Handle API errors and display field-level errors
  - On success, show success message and redirect to invoice detail page or clear form for next entry
  - Provide "Save" and "Save & Create Another" buttons

### 10. Integrate API Client Functions
- Add to `app/client/src/api/client.ts`:
  - `searchCustomers(query: string): Promise<Customer[]>` - Call GET /api/customers/search
  - `createManualInvoice(invoiceData: ManualInvoiceCreate): Promise<Invoice>` - Call POST /api/invoices/manual
  - Add error handling and response parsing
  - Add request timeout (30 seconds)

### 11. Add Navigation and Routing
- Update `app/client/src/App.tsx`:
  - Add route `/invoices/manual` for ManualInvoiceEntry component
  - Add "Manual Entry" button to invoice list page header
  - Add breadcrumb navigation (Home > Invoices > Manual Entry)
- Update invoice list component to include link to manual entry page

### 12. Implement Duplicate Detection UI Flow
- When duplicate invoice_number detected (409 response from API):
  - Show modal dialog with duplicate invoice details
  - Options: "Update Existing Invoice", "Use Different Invoice Number", "Cancel"
  - If "Update Existing", populate form with existing data for editing
  - If "Use Different Invoice Number", highlight invoice_number field and prompt for change
  - If "Cancel", close modal and stay on form

### 13. Add Form Reset and Multi-Entry Features
- Implement "Clear Form" button that resets all fields to initial state
- Implement "Save & Create Another" button that:
  - Saves current invoice
  - Shows success message briefly
  - Clears form for next entry
  - Keeps user on manual entry page for efficient batch data entry
- Add keyboard shortcut (Ctrl+S / Cmd+S) to save form

### 14. Create E2E Test for Manual Invoice Entry
- Create `.claude/commands/e2e/test_manual_invoice_entry.md`:
  - Navigate to manual invoice entry page
  - Fill in invoice number, date
  - Search for existing customer using autocomplete
  - Select customer from dropdown
  - Add 3 line items with products, quantities, prices
  - Verify totals are calculated correctly
  - Click Save button
  - Verify success message appears
  - Verify redirect to invoice detail page
  - Verify invoice appears in invoice list
  - Take screenshots at each step

### 15. Create E2E Test for Duplicate Detection
- Create `.claude/commands/e2e/test_duplicate_detection.md`:
  - Navigate to manual invoice entry page
  - Enter invoice number that already exists in database
  - Fill in other required fields
  - Click Save button
  - Verify duplicate detection error appears
  - Verify error message shows existing invoice details
  - Click "Use Different Invoice Number"
  - Change invoice number to unique value
  - Click Save button
  - Verify invoice is created successfully
  - Take screenshots showing duplicate detection and resolution

### 16. Create E2E Test for Customer Autocomplete
- Create `.claude/commands/e2e/test_customer_autocomplete.md`:
  - Navigate to manual invoice entry page
  - Click into customer name field
  - Type partial customer name (e.g., "Acme")
  - Verify dropdown appears with matching customers
  - Verify list updates as user types
  - Use keyboard arrow down to navigate list
  - Press Enter to select customer
  - Verify customer fields are populated (name, address, phone, email)
  - Take screenshots showing autocomplete behavior

### 17. Add Styling and Responsive Design
- Add CSS styles for manual invoice entry form in `app/client/src/style.css`
- Form layout: two-column layout on desktop, single column on mobile/tablet
- Clear visual separation between form sections
- Highlight required fields with asterisk or bold label
- Error messages in red below invalid fields
- Success message in green banner at top
- Responsive design for mobile devices
- Loading spinner overlay during API calls
- Disabled state for form inputs during submission

### 18. Run Validation Commands
- Execute all validation commands to ensure zero regressions
- Fix any failing tests or validation errors
- Verify E2E tests pass with screenshots

## Testing Strategy

### Unit Tests

#### Backend Unit Tests
- **Validator Tests** (`test_validators.py`):
  - Test `validate_invoice_number` with valid and invalid formats
  - Test `validate_date` with various date formats, future dates, invalid dates
  - Test `validate_amount` with positive, negative, zero, and non-numeric values
  - Test `validate_email` with valid and invalid email addresses
  - Test `validate_phone` with various phone number formats
  - Test `validate_address` with complete and incomplete addresses
  - Test `check_duplicate_invoice` with existing and new invoice numbers

- **Manual Invoice CRUD Tests** (`test_manual_invoice.py`):
  - Test POST /api/invoices/manual with valid complete data
  - Test POST /api/invoices/manual with missing required fields (expect 422)
  - Test POST /api/invoices/manual with duplicate invoice_number (expect 409)
  - Test POST /api/invoices/manual with invalid customer_id (expect 404)
  - Test POST /api/invoices/manual with new customer name (should create customer)
  - Test POST /api/invoices/manual with multiple line items
  - Test POST /api/invoices/manual with invalid line item data
  - Test GET /api/customers/search with partial name match
  - Test GET /api/customers/search with no matches

#### Frontend Unit Tests
- **useInvoiceForm Hook Tests**:
  - Test initial form state
  - Test add line item function
  - Test remove line item function
  - Test total calculation when line items change
  - Test form reset function

- **useCustomerSearch Hook Tests**:
  - Test debounced search triggers API call
  - Test keyboard navigation updates selected index
  - Test select customer updates form state

- **Component Tests**:
  - Test LineItemForm renders correct number of rows
  - Test LineItemForm calculates totals correctly
  - Test CustomerAutocomplete shows dropdown on input
  - Test ManualInvoiceEntry submits correct data

### Edge Cases

1. **Empty Fields**: Test form submission with required fields empty - should show validation errors
2. **Invalid Data Formats**:
   - Invalid date format (e.g., "32/13/2024") - should show error
   - Non-numeric amounts (e.g., "abc") - should show error
   - Invalid email format - should show error
3. **Duplicate Invoice Number**: Enter existing invoice_number - should show duplicate detection dialog
4. **Network Errors**:
   - Simulate API timeout - should show timeout error
   - Simulate 500 server error - should show generic error message
5. **Very Long Customer Names**: Test with 500+ character customer name - should handle gracefully
6. **Special Characters**: Test with customer names containing Unicode, emojis, special chars
7. **Many Line Items**: Test form with 50+ line items - should handle performance
8. **Zero Quantities**: Test line item with quantity = 0 - should allow or show validation error
9. **Negative Amounts**: Test line item with negative price - should show validation error
10. **Large Numbers**: Test with amounts in millions/billions - should format correctly
11. **Concurrent Submissions**: Click Save button multiple times rapidly - should prevent duplicate submissions
12. **Browser Refresh**: Fill form halfway and refresh - should warn about unsaved changes
13. **Customer Not Found**: Search for customer that doesn't exist - should show "Create New" option
14. **Autocomplete Keyboard Navigation**: Test arrow keys and enter key in customer autocomplete
15. **Mobile Device**: Test form on mobile screen size - should be usable with touch input

### Integration Tests

- Test complete flow: search customer → fill form → add line items → save → verify in database
- Test duplicate flow: enter duplicate invoice → see error → change number → save successfully
- Test new customer flow: enter new customer name → fill invoice → save → verify customer created
- Test edit flow from duplicate: detect duplicate → choose "Update Existing" → modify data → save

## Acceptance Criteria

1. Manual invoice entry form is accessible from main invoice list page via "Manual Entry" button
2. Form includes sections for invoice information, customer information, billing address, line items, and notes
3. Customer autocomplete suggests existing customers as user types (debounced search)
4. Customer autocomplete allows manual entry if customer not found
5. Line items can be dynamically added and removed (Add Row / Remove Row buttons)
6. Line item totals are calculated automatically (quantity × unit_price)
7. Invoice total is calculated automatically (sum of all line item totals)
8. Client-side validation shows errors immediately for:
   - Required fields (invoice_number, customer_name, date)
   - Date format (MM/DD/YYYY or similar)
   - Numeric amounts (must be positive numbers)
   - Email format (if provided)
9. Server-side validation returns field-level error messages for invalid data
10. Duplicate invoice_number detection shows error with options to update or use different number
11. Successful save shows confirmation message and either:
    - Redirects to invoice detail page, OR
    - Clears form for next entry (if "Save & Create Another" clicked)
12. All data is saved to database with proper relationships (Invoice → LineItems, Invoice → Customer)
13. Manually entered invoices appear in main invoice list alongside automatically extracted invoices
14. Form is responsive and usable on desktop, tablet, and mobile devices
15. All unit tests pass with >80% code coverage for new code
16. All 3 E2E tests pass with screenshots:
    - Manual invoice entry complete flow
    - Duplicate detection and resolution
    - Customer autocomplete functionality
17. Form handles errors gracefully with user-friendly messages (no stack traces shown to user)
18. Form prevents duplicate submissions (disable button during API call)
19. Clear Form button resets all fields to initial state
20. API response time <500ms for manual invoice creation with 10 line items

## Validation Commands
Execute every command to validate the feature works correctly with zero regressions.

- `cd app/server && uv run pytest -v --cov=. --cov-report=term-missing` - Run server tests including new manual invoice tests
- `cd app/server && uv run pytest test_validators.py -v` - Run validator unit tests specifically
- `cd app/server && uv run pytest test_manual_invoice.py -v` - Run manual invoice API tests specifically
- `cd app/client && bun tsc --noEmit` - Run TypeScript type checking to validate no type errors
- `cd app/client && bun run build` - Run frontend build to validate production build succeeds
- Read `.claude/commands/test_e2e.md`, then read and execute `.claude/commands/e2e/test_manual_invoice_entry.md` to validate complete manual entry flow
- Read `.claude/commands/test_e2e.md`, then read and execute `.claude/commands/e2e/test_duplicate_detection.md` to validate duplicate detection works
- Read `.claude/commands/test_e2e.md`, then read and execute `.claude/commands/e2e/test_customer_autocomplete.md` to validate autocomplete functionality
- `curl -X POST http://localhost:8000/api/invoices/manual -H "Content-Type: application/json" -d @sample/manual_invoice_payload.json` - Test manual invoice creation via curl
- `curl http://localhost:8000/api/customers/search?q=Acme` - Test customer search API

## Notes

### Dependencies
All dependencies should already be installed from Issue #1 implementation. If not, add using `uv add`:

**Backend**: No new dependencies required (uses existing FastAPI, SQLAlchemy, Pydantic)

**Frontend**: No new dependencies required (uses existing React, TypeScript, fetch API)

### Design Considerations

#### Form UX
- Use clear labels and placeholders for all fields
- Mark required fields with asterisk (*)
- Show validation errors inline below each field in red
- Disable Save button while form is invalid or submitting
- Show loading spinner during API call
- Auto-focus first field on page load
- Tab order follows logical form flow

#### Data Consistency
- Manual entries must follow same schema as automated extraction
- Use same CRUD operations and database models
- Maintain referential integrity (foreign keys, NOT NULL constraints)
- Apply same business rules (e.g., invoice date validation, amount calculations)

#### Error Handling
- Network errors: "Unable to connect to server. Please check your connection and try again."
- Validation errors: Show specific field errors (e.g., "Invoice date cannot be in the future")
- Duplicate errors: "Invoice #12345 already exists. Would you like to update it or use a different invoice number?"
- Server errors: "An unexpected error occurred. Please try again. If the problem persists, contact support."

#### Performance
- Debounce customer autocomplete search (300ms) to reduce API calls
- Limit autocomplete results to 10 customers
- Lazy load customer details only when selected
- Use form memo/useMemo to prevent unnecessary re-renders
- Validate on blur, not on every keystroke

#### Accessibility
- Use semantic HTML (labels, fieldsets, legends)
- Ensure keyboard navigation works (tab, enter, arrow keys)
- Add ARIA labels for screen readers
- Use sufficient color contrast for text and errors
- Support keyboard shortcuts (Ctrl+S to save)

### Future Enhancements

1. **Bulk Manual Entry**: Allow uploading CSV with multiple invoices for manual bulk entry
2. **Templates**: Save invoice templates for frequently entered invoice types (same customer, products)
3. **Draft Saving**: Auto-save draft invoices to prevent data loss on browser crash
4. **Audit Trail**: Track who manually entered each invoice and when
5. **Invoice Editing**: Allow editing of manually entered invoices (currently create-only)
6. **Copy Invoice**: Duplicate existing invoice to quickly create similar invoice
7. **Barcode Scanner**: Integrate barcode scanner for quick product entry
8. **Tax Calculation**: Auto-calculate tax based on customer location and product type
9. **Multi-Currency**: Support invoices in different currencies with conversion
10. **Attachments**: Allow uploading supporting documents (receipts, POs) with manual entry
11. **Approval Workflow**: Route manually entered invoices through approval process before finalizing
12. **Batch Operations**: Select multiple draft invoices and submit all at once

### Integration with Automated System

The manual entry system is designed to complement the automated PDF/Word extraction system:
- Both systems write to the same database tables
- Both use the same data validation rules
- Manual entries have source indicator (source='manual' vs source='automated')
- Manual entries can override automated extraction results
- Manual entries participate in same analytics and reporting
- Duplicate detection works across both manual and automated entries

### Security Considerations

- Input validation prevents SQL injection (using SQLAlchemy ORM)
- XSS prevention (React escapes user input by default)
- CSRF protection (if using session-based auth)
- Rate limiting on manual invoice creation (prevent abuse)
- Audit logging for manual entries (who, when, what)
- Role-based access control (only authorized users can manually enter invoices)
