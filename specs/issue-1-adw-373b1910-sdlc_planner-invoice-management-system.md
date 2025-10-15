# Feature: Invoice Management System

## Metadata
issue_number: `1`
adw_id: `373b1910`
issue_json: `{"number":1,"title":"Build invoice management application with database and web interface","body":"## Feature Request\n\nBuild a full-stack invoice management system to organize and query the invoice PDFs stored in the invoices/ directory.\n\n## Requirements\n\n### Backend\n- SQLite database to store invoice metadata\n- Python/FastAPI server with REST API endpoints\n- PDF parsing to extract invoice data (invoice number, date, amount, vendor, etc.)\n- Bulk import functionality to process all existing invoices\n- Search and filter capabilities\n\n### Frontend  \n- Modern web interface (React/Vue/HTML+JS)\n- Invoice list view with search/filter\n- Individual invoice detail view\n- PDF viewer integration\n- Export functionality (CSV, JSON)\n\n### Database Schema\n- invoices table with fields:\n  - id (primary key)\n  - invoice_number (unique)\n  - date\n  - vendor_name\n  - amount\n  - file_path\n  - created_at\n  - updated_at\n\n### Nice to Have\n- Invoice analytics dashboard\n- Duplicate detection\n- OCR for scanned invoices\n- Multi-user support with authentication\n\n## Context\nWe have 6 directories of invoice PDFs:\n- Invoices 10220-10400\n- Invoices 10221-13299\n- Invoices 13300-13399\n- Invoices 13400-13499\n- Invoices 13500-13520\n- Invoices 13530-13630\n\nThe application should be able to scan these directories and import all invoice data into the database.\n\n## Success Criteria\n- All invoices successfully imported to database\n- Web interface allows searching/filtering invoices\n- Can view invoice details and PDF\n- Application is production-ready with error handling and logging"}`

## Feature Description
This feature implements a comprehensive full-stack invoice management system designed to organize, search, and manage invoice PDFs stored across multiple directories. The system consists of a Python/FastAPI backend with SQLite database storage, PDF parsing capabilities, and a modern web interface for browsing, searching, filtering, and viewing invoice data. The application enables users to import bulk invoices, extract metadata automatically, search and filter invoices, view PDF documents, and export data in various formats.

## User Story
As a business user managing thousands of invoice PDFs across multiple directories
I want to organize, search, and query invoice data through a web interface
So that I can efficiently access invoice information, track vendor payments, and analyze invoice data without manually opening individual PDF files

## Problem Statement
The organization has approximately 3,000+ invoice PDFs scattered across 6 different directories (ranging from invoice numbers 10220 to 13630). Currently, there is no centralized system to:
- Search for specific invoices by vendor, date, amount, or invoice number
- View invoice metadata without opening individual PDFs
- Track and analyze invoice data over time
- Export invoice data for reporting or integration with other systems
- Detect duplicate invoices
- Access invoices through a user-friendly web interface

This lack of organization leads to inefficient workflows, difficulty in financial tracking, and inability to perform bulk operations or analytics on invoice data.

## Solution Statement
Build a full-stack invoice management system with:
1. **Backend Infrastructure**: Python/FastAPI server with SQLite database for storing invoice metadata, PDF parsing engine using PyPDF2/pdfplumber for extracting invoice data, and RESTful API endpoints for CRUD operations
2. **Data Import System**: Automated bulk import functionality to scan all 6 invoice directories, extract metadata (invoice number, date, vendor, amount), and populate the database with error handling and progress tracking
3. **Modern Web Interface**: React-based frontend with invoice list view, advanced search/filter capabilities, individual invoice detail pages with embedded PDF viewer, and data export functionality (CSV/JSON)
4. **Enhanced Features**: Duplicate detection, invoice analytics dashboard, comprehensive error handling, and logging for production readiness

The solution provides a centralized, searchable repository for all invoice data with an intuitive interface, enabling efficient invoice management and data analysis.

## Relevant Files
Use these files to implement the feature:

Since this is a new project, we need to establish the complete application structure. The following files will be created:

### New Files

#### Backend Files
- `app/server/main.py` - FastAPI application entry point with API routes
- `app/server/database.py` - SQLite database connection and session management
- `app/server/models.py` - SQLAlchemy models for invoices table
- `app/server/schemas.py` - Pydantic schemas for request/response validation
- `app/server/crud.py` - CRUD operations for invoice management
- `app/server/pdf_parser.py` - PDF parsing logic to extract invoice metadata
- `app/server/importer.py` - Bulk import functionality for scanning directories and importing invoices
- `app/server/config.py` - Configuration management for application settings
- `app/server/utils.py` - Utility functions for logging, error handling
- `app/server/requirements.txt` - Python dependencies
- `app/server/pyproject.toml` - UV project configuration
- `app/server/pytest.ini` - Pytest configuration
- `app/server/test_main.py` - Tests for API endpoints
- `app/server/test_pdf_parser.py` - Tests for PDF parsing functionality
- `app/server/test_importer.py` - Tests for bulk import functionality

#### Frontend Files
- `app/client/package.json` - Node.js dependencies and scripts
- `app/client/vite.config.ts` - Vite build configuration
- `app/client/tsconfig.json` - TypeScript configuration
- `app/client/index.html` - HTML entry point
- `app/client/src/main.tsx` - React application entry point
- `app/client/src/App.tsx` - Root React component with routing
- `app/client/src/style.css` - Global styles
- `app/client/src/api/client.ts` - API client for backend communication
- `app/client/src/types/invoice.ts` - TypeScript interfaces for invoice data
- `app/client/src/components/InvoiceList.tsx` - Invoice list view with search/filter
- `app/client/src/components/InvoiceDetail.tsx` - Individual invoice detail view
- `app/client/src/components/PdfViewer.tsx` - PDF viewer component
- `app/client/src/components/SearchBar.tsx` - Search and filter component
- `app/client/src/components/ExportButton.tsx` - Export functionality (CSV/JSON)
- `app/client/src/components/ImportButton.tsx` - Trigger bulk import
- `app/client/src/components/Analytics.tsx` - Invoice analytics dashboard

#### Scripts Files
- `scripts/start_server.sh` - Script to start FastAPI server
- `scripts/start_client.sh` - Script to start Vite dev server
- `scripts/stop_server.sh` - Script to stop FastAPI server
- `scripts/stop_client.sh` - Script to stop Vite dev server
- `scripts/import_invoices.sh` - Script to trigger bulk import

#### Configuration Files
- `README.md` - Project documentation with setup instructions
- `.env.sample` - Sample environment variables (updated with invoice paths)
- `pyproject.toml` - Root project configuration (if needed)

#### E2E Test Files
- `.claude/commands/e2e/test_invoice_search.md` - E2E test for searching and filtering invoices
- `.claude/commands/e2e/test_invoice_detail.md` - E2E test for viewing invoice details and PDF
- `.claude/commands/e2e/test_invoice_export.md` - E2E test for exporting invoice data

#### Documentation Files
- Need to read `.claude/commands/test_e2e.md` to understand E2E test execution framework
- Need to read `.claude/commands/e2e/test_basic_query.md` as an example of E2E test structure
- Need to read `app_docs/feature-490eb6b5-one-click-table-exports.md` for CSV export patterns (if it exists)

## Implementation Plan
### Phase 1: Foundation
1. **Project Setup**: Initialize the application structure with app/server and app/client directories, configure UV for Python dependency management, configure Vite with React and TypeScript for frontend
2. **Database Design**: Create SQLAlchemy models for invoices table with all required fields (id, invoice_number, date, vendor_name, amount, file_path, created_at, updated_at), set up SQLite database connection with proper session management
3. **Configuration Management**: Set up environment variables for database path, invoice directories, API port, and logging configuration
4. **Basic API Structure**: Create FastAPI application with CORS middleware, health check endpoint, and basic error handling

### Phase 2: Core Implementation
1. **PDF Parsing Engine**: Implement PDF text extraction using PyPDF2/pdfplumber, create regex patterns to extract invoice number, date, vendor, and amount from PDF text, handle various PDF formats and layouts with error handling for unreadable PDFs
2. **Bulk Import System**: Implement directory scanner to find all PDF files in specified directories, create batch processing logic with progress tracking and error logging, implement duplicate detection based on invoice_number, add database transaction handling for bulk inserts
3. **CRUD API Endpoints**: Implement GET /api/invoices (list with pagination, search, filter), GET /api/invoices/{id} (single invoice detail), POST /api/invoices/import (trigger bulk import), GET /api/invoices/export (CSV/JSON export), GET /api/invoices/stats (analytics data)
4. **Frontend Components**: Create InvoiceList component with search bar, filters (date range, vendor, amount range), and pagination, create InvoiceDetail component with metadata display and PDF viewer integration, create SearchBar component with real-time search, create ExportButton component with format selection

### Phase 3: Integration
1. **API Integration**: Connect frontend to backend API using fetch/axios, implement error handling and loading states, add retry logic for failed requests
2. **PDF Viewing**: Integrate PDF viewer library (e.g., react-pdf) to display PDFs in the browser, implement PDF loading states and error handling
3. **Export Functionality**: Implement CSV export using pandas on backend, implement JSON export with proper formatting, add download functionality in frontend
4. **Analytics Dashboard**: Create analytics component showing total invoices, total amount, top vendors, monthly trends, use charts library (e.g., recharts) for data visualization
5. **Production Readiness**: Add comprehensive error handling throughout application, implement logging with rotation, add input validation on all endpoints, implement rate limiting, add health check endpoints for monitoring

## Step by Step Tasks
IMPORTANT: Execute every step in order, top to bottom.

### 1. Initialize Project Structure and Configuration
- Create app/server and app/client directory structure
- Initialize UV project in app/server with pyproject.toml
- Initialize Vite + React + TypeScript project in app/client
- Create scripts directory with start/stop scripts for server and client
- Update .env.sample with invoice directory paths and configuration
- Create README.md with project overview and setup instructions

### 2. Set Up Backend Database and Models
- Create app/server/database.py with SQLite connection and session management
- Create app/server/models.py with Invoice SQLAlchemy model (id, invoice_number, date, vendor_name, amount, file_path, created_at, updated_at)
- Create app/server/schemas.py with Pydantic schemas for Invoice (InvoiceBase, InvoiceCreate, InvoiceResponse)
- Create app/server/config.py for environment variable management
- Add database initialization and migration logic
- Write unit tests for models in app/server/test_models.py

### 3. Implement PDF Parsing Engine
- Create app/server/pdf_parser.py with PDF text extraction using pdfplumber
- Implement extract_invoice_data function with regex patterns for invoice_number, date, vendor_name, amount
- Handle various PDF formats and layouts with fallback strategies
- Add comprehensive error handling for corrupted or unreadable PDFs
- Write unit tests for PDF parsing in app/server/test_pdf_parser.py with sample PDFs
- Test parsing with actual invoice PDFs from the directories

### 4. Create Bulk Import System
- Create app/server/importer.py with directory scanning functionality
- Implement batch processing with progress tracking (processed count, failed count)
- Add duplicate detection based on invoice_number before insertion
- Implement database transaction handling for bulk inserts with rollback on errors
- Add logging for import process (started, completed, errors)
- Write unit tests for import logic in app/server/test_importer.py

### 5. Build FastAPI REST API
- Create app/server/main.py with FastAPI application initialization
- Set up CORS middleware for frontend communication
- Create app/server/crud.py with database operations (get_invoices, get_invoice, create_invoice, search_invoices)
- Implement GET /api/invoices endpoint with pagination, search, and filter parameters
- Implement GET /api/invoices/{id} endpoint for single invoice retrieval
- Implement POST /api/invoices/import endpoint to trigger bulk import
- Implement GET /api/invoices/export endpoint for CSV/JSON export
- Implement GET /api/invoices/stats endpoint for analytics data
- Implement GET /health endpoint for health checks
- Add request validation and error handling middleware
- Write API integration tests in app/server/test_main.py

### 6. Create Frontend Foundation
- Initialize React + TypeScript project with Vite in app/client
- Create app/client/src/types/invoice.ts with Invoice interface
- Create app/client/src/api/client.ts with API client functions (fetchInvoices, fetchInvoiceById, triggerImport, exportData, fetchStats)
- Set up React Router for navigation (list view, detail view)
- Create app/client/src/style.css with base styles
- Create app/client/src/App.tsx as root component with routing

### 7. Build Invoice List Component
- Create app/client/src/components/InvoiceList.tsx with table/card view for invoices
- Add pagination controls (previous, next, page numbers)
- Add sorting functionality (by date, amount, vendor)
- Implement loading and error states
- Add empty state when no invoices found
- Style component with responsive design

### 8. Build Search and Filter Component
- Create app/client/src/components/SearchBar.tsx with search input
- Add filter controls for date range (start date, end date)
- Add filter controls for vendor (dropdown or autocomplete)
- Add filter controls for amount range (min, max)
- Implement debounced search to reduce API calls
- Connect filters to API query parameters

### 9. Build Invoice Detail Component
- Create app/client/src/components/InvoiceDetail.tsx for displaying single invoice
- Display all invoice metadata (invoice_number, date, vendor, amount)
- Add back button to return to list view
- Implement loading and error states
- Style component with clear layout

### 10. Integrate PDF Viewer
- Install react-pdf library in app/client
- Create app/client/src/components/PdfViewer.tsx wrapper component
- Implement PDF loading from file_path
- Add zoom controls and page navigation
- Handle PDF loading errors gracefully
- Test with various PDF sizes and formats

### 11. Build Export Functionality
- Create app/client/src/components/ExportButton.tsx with format dropdown (CSV, JSON)
- Implement CSV export on backend using pandas
- Implement JSON export with proper formatting
- Add download trigger in frontend
- Test export with large datasets
- Add export progress indicator

### 12. Create Import Trigger UI
- Create app/client/src/components/ImportButton.tsx to trigger bulk import
- Show import progress (processed count, failed count)
- Display success/error messages after import completes
- Add confirmation dialog before triggering import
- Test import with all 6 invoice directories

### 13. Build Analytics Dashboard
- Create app/client/src/components/Analytics.tsx for analytics view
- Implement backend endpoint for aggregated stats (total invoices, total amount, top vendors, monthly breakdown)
- Display key metrics (total invoices, total amount, average amount)
- Create charts for top vendors (bar chart)
- Create charts for monthly trends (line chart)
- Add date range filter for analytics

### 14. Add Production-Ready Features
- Implement comprehensive error handling throughout backend and frontend
- Set up logging with rotation in app/server/utils.py
- Add input validation on all API endpoints
- Implement API rate limiting
- Add request timeout handling
- Create comprehensive error messages for users
- Add health check monitoring

### 15. Create E2E Tests
- Create `.claude/commands/e2e/test_invoice_search.md` with steps to:
  - Navigate to invoice list
  - Enter search query for specific vendor
  - Apply date range filter
  - Verify filtered results appear
  - Take screenshots of search results
- Create `.claude/commands/e2e/test_invoice_detail.md` with steps to:
  - Navigate to invoice list
  - Click on specific invoice
  - Verify detail page loads with correct data
  - Verify PDF viewer displays the invoice
  - Take screenshots of detail view and PDF
- Create `.claude/commands/e2e/test_invoice_export.md` with steps to:
  - Navigate to invoice list
  - Click export button
  - Select CSV format
  - Verify download starts
  - Take screenshot of export action

### 16. Create Scripts and Documentation
- Create scripts/start_server.sh to start FastAPI with uvicorn
- Create scripts/start_client.sh to start Vite dev server
- Create scripts/stop_server.sh to stop FastAPI server
- Create scripts/stop_client.sh to stop Vite dev server
- Create scripts/import_invoices.sh to trigger bulk import via API
- Make all scripts executable
- Update README.md with complete setup instructions, API documentation, and usage examples

### 17. Run Validation Commands
- Execute all validation commands listed below to ensure zero regressions
- Fix any failing tests or build errors
- Verify E2E tests pass with screenshots
- Confirm application is production-ready

## Testing Strategy
### Unit Tests
- **PDF Parser Tests**: Test extract_invoice_data with sample PDFs containing various formats, test error handling for corrupted PDFs, test regex patterns for invoice_number, date, vendor_name, amount extraction
- **Importer Tests**: Test directory scanning with mock file system, test batch processing logic, test duplicate detection, test error handling for failed imports
- **API Endpoint Tests**: Test GET /api/invoices with various query parameters (search, filter, pagination), test GET /api/invoices/{id} with valid and invalid IDs, test POST /api/invoices/import, test GET /api/invoices/export for CSV and JSON formats, test GET /api/invoices/stats
- **CRUD Tests**: Test database operations (create, read, update, delete), test search and filter logic, test pagination logic
- **Frontend Component Tests**: Test InvoiceList rendering and interaction, test SearchBar filter state management, test InvoiceDetail data display, test ExportButton download trigger

### Edge Cases
- **Empty database**: Verify application handles no invoices gracefully with appropriate empty state
- **Large datasets**: Test pagination and performance with 3000+ invoices
- **Invalid PDFs**: Test error handling for corrupted, password-protected, or non-text PDFs
- **Duplicate invoices**: Verify duplicate detection prevents duplicate entries
- **Missing metadata**: Test handling when PDF parsing cannot extract some fields (use null/default values)
- **Concurrent imports**: Test what happens if bulk import is triggered while another import is running
- **Invalid search queries**: Test API with malformed search parameters
- **Network failures**: Test frontend error handling when API is unavailable
- **Large PDF files**: Test PDF viewer with multi-page and large file size PDFs
- **Special characters**: Test vendor names and invoice numbers with special characters
- **Date formats**: Test various date formats in PDFs (MM/DD/YYYY, DD/MM/YYYY, etc.)
- **Export large datasets**: Test CSV/JSON export with 1000+ invoices

## Acceptance Criteria
1. All 6 invoice directories are successfully scanned and invoices imported into SQLite database
2. Invoice list view displays all invoices with pagination (25 per page)
3. Search functionality works for invoice_number, vendor_name with partial matches
4. Filter functionality works for date range and amount range
5. Individual invoice detail page displays all metadata fields correctly
6. PDF viewer successfully displays invoice PDFs inline
7. Export to CSV produces valid CSV file with all invoice data
8. Export to JSON produces valid JSON file with all invoice data
9. Analytics dashboard displays accurate statistics (total count, total amount, top vendors, monthly trends)
10. Duplicate invoices are detected and not imported (based on invoice_number)
11. All unit tests pass with >80% code coverage
12. All E2E tests pass with screenshots validating functionality
13. Application handles errors gracefully with user-friendly messages
14. Application logs all important events (imports, errors, API requests)
15. API responds within 500ms for list queries with <100 results
16. Frontend is responsive and works on desktop and tablet devices
17. README.md contains complete setup and usage instructions
18. Application can be started with simple script commands (start_server.sh, start_client.sh)

## Validation Commands
Execute every command to validate the feature works correctly with zero regressions.

- `cd app/server && uv run pytest -v --cov=. --cov-report=term-missing` - Run server tests with coverage to validate backend functionality with zero regressions
- `cd app/client && bun tsc --noEmit` - Run TypeScript type checking to validate frontend has no type errors
- `cd app/client && bun run build` - Run frontend build to validate production build succeeds with zero errors
- `cd app/server && uv run python -m app.server.importer` - Test bulk import on sample invoice directory to validate PDF parsing and database insertion
- `bash scripts/start_server.sh` - Start FastAPI server and verify it starts successfully
- `bash scripts/start_client.sh` - Start Vite dev server and verify it starts successfully
- `curl http://localhost:8000/health` - Verify health check endpoint returns 200 OK
- `curl http://localhost:8000/api/invoices?limit=10` - Verify invoice list API returns data
- Read `.claude/commands/test_e2e.md`, then read and execute `.claude/commands/e2e/test_invoice_search.md` to validate search and filter functionality works
- Read `.claude/commands/test_e2e.md`, then read and execute `.claude/commands/e2e/test_invoice_detail.md` to validate invoice detail view and PDF viewer works
- Read `.claude/commands/test_e2e.md`, then read and execute `.claude/commands/e2e/test_invoice_export.md` to validate export functionality works
- `bash scripts/stop_server.sh` - Stop server after validation
- `bash scripts/stop_client.sh` - Stop client after validation

## Notes
### Additional Dependencies
The following libraries will be added using `uv add`:
- **Backend**: fastapi, uvicorn[standard], sqlalchemy, pydantic, pydantic-settings, python-multipart, pdfplumber (or PyPDF2), pandas (for CSV export), python-dotenv, pytest, pytest-cov, httpx (for testing)
- **Frontend**: react, react-dom, react-router-dom, @types/react, @types/react-dom, typescript, vite, react-pdf (for PDF viewing), recharts (for analytics charts), axios (for API calls)

### Future Considerations
- **OCR Integration**: For scanned invoices without extractable text, integrate OCR library (e.g., pytesseract, Azure Computer Vision) to extract text before parsing
- **Multi-user Authentication**: Add user accounts with JWT authentication, role-based access control (admin, user), and user-specific invoice views
- **Invoice Editing**: Allow users to manually edit invoice metadata when PDF parsing is incorrect
- **Advanced Analytics**: Add more analytics views (vendor comparison, seasonal trends, expense categories)
- **Cloud Storage**: Integrate with S3 or similar for storing PDFs instead of local file system
- **Email Notifications**: Send notifications when bulk import completes or when errors occur
- **Invoice Approval Workflow**: Add approval status field and workflow for invoice processing
- **API Documentation**: Generate OpenAPI/Swagger documentation for API endpoints
- **Database Migrations**: Implement proper database migration system (Alembic) for schema changes
- **Caching**: Add Redis caching for frequently accessed invoices and search results
- **Full-text Search**: Integrate full-text search engine (Elasticsearch) for better search performance

### Design Patterns
- **Backend**: Use repository pattern for database operations, dependency injection for database sessions, service layer for business logic, DTOs (Pydantic schemas) for data validation
- **Frontend**: Use React hooks for state management, custom hooks for API calls, component composition for reusability, error boundaries for error handling
- **Error Handling**: Consistent error response format across all API endpoints, user-friendly error messages in frontend, comprehensive logging on backend

### Performance Considerations
- **Pagination**: Implement cursor-based pagination for large datasets
- **Lazy Loading**: Load PDFs on-demand, not all at once
- **Indexing**: Add database indexes on frequently queried fields (invoice_number, date, vendor_name)
- **Batch Processing**: Process invoice imports in batches of 100 to avoid memory issues
- **Debouncing**: Debounce search input to reduce API calls

### Security Considerations
- **Input Validation**: Validate all user inputs to prevent SQL injection and XSS attacks
- **File Access**: Ensure PDF file paths are validated to prevent directory traversal attacks
- **CORS**: Configure CORS to only allow requests from trusted origins
- **Rate Limiting**: Implement rate limiting to prevent abuse
- **SQL Injection**: Use parameterized queries through SQLAlchemy ORM (already safe)
