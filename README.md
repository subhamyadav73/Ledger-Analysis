# Ledger-Analysis

## Overview
Ledger-Analysis is a comprehensive data analysis project that processes financial ledger transactions and performs intelligent categorization, frequency analysis, and duplicate detection using advanced fuzzy matching and Levenshtein distance algorithms.

## Project Purpose
This project analyzes credit transaction data from an Excel file to:
- Group and categorize ledger entries
- Detect similar ledger names using fuzzy matching algorithms
- Calculate transaction frequencies at various levels (company, group, ledger)
- Identify ledgers with low transaction frequencies
- Generate clean, organized Excel reports for business analysis

## Files and Descriptions

### Ledger_analysis_1.ipynb
**Purpose:** Group-wise and Company-wise Ledger Frequency Analysis
- Imports transaction data from Excel
- Groups transactions by company and direct group
- Calculates ledger frequency statistics
- Identifies ledgers with above-average frequencies
- Exports results to multiple Excel sheets organized by group and company

**Key Outputs:**
- `ledger_freq_group-wise_sheetname_as_group.xlsx` - Ledger frequency grouped by direct group
- `ledger_freq_company-wise.xlsx` - Ledger frequency by company
- `ledger_freq_group-wise.xlsx` - Consolidated group-wise ledger frequency

### Ledger_analysis_2.ipynb
**Purpose:** Company Frequency per Ledger Analysis
- Analyzes how many unique companies use each ledger
- Cleans illegal Excel characters from ledger names
- Calculates unique company count per ledger name
- Sorts results by company frequency in descending order

**Key Outputs:**
- `output_file.xlsx` - Ledger names with associated company frequency counts

### Ledger_analysis_3.ipynb
**Purpose:** Fuzzy Matching Ledger Categorization (RapidFuzz)
- Normalizes ledger names (lowercase, removes special characters, handles duplicate chars)
- Uses RapidFuzz library with token_sort_ratio for similarity matching
- Groups similar ledgers into categories (90% similarity threshold)
- Filters low-frequency ledgers
- Creates normalized "Ledger Category" field

**Key Outputs:**
- Enhanced dataframe with `Ledger Category` column
- Consolidated ledger names based on 90% similarity threshold

### Ledger_analysis_4.ipynb
**Purpose:** Advanced Levenshtein Distance Based Ledger Clustering
- Implements custom Levenshtein distance similarity function (82% threshold)
- Uses blocking key optimization (first 4 characters) for performance
- Clusters ledgers by frequency and similarity
- Marks low-frequency ledgers (< 5 transactions) as "OTHER"
- Cleans illegal Excel control characters
- Exports final categorized dataset

**Key Outputs:**
- `output_file.xlsx` - Final cleaned and categorized transaction data

## Dependencies

```
pandas>=1.3.0      # Data manipulation and Excel I/O
openpyxl>=3.6.0    # Excel file writing with sheet management
rapidfuzz>=2.0.0   # Fuzzy string matching (used in Ledger_analysis_3.ipynb)
re                 # Regular expressions (built-in)
```

## Installation

1. Clone the repository:
```bash
git clone https://github.com/subhamyadav73/Ledger-Analysis.git
cd Ledger-Analysis
```

2. Install required packages:
```bash
pip install pandas openpyxl rapidfuzz
```

## Usage

1. Place your transaction data file in the project root directory

2. Run the notebooks in sequence:
   - **Ledger_analysis_1.ipynb** - For group-wise and company-wise frequency analysis
   - **Ledger_analysis_2.ipynb** - For company frequency per ledger
   - **Ledger_analysis_3.ipynb** - For fuzzy matching categorization (RapidFuzz approach)
   - **Ledger_analysis_4.ipynb** - For final Levenshtein-based clustering and export

3. View the generated Excel files for analysis results

## Input Data Format

The input file should contain the following columns:
- `Ledger Name` - Name of the ledger account
- `Actual Ledger Name` - Alternate ledger identifier
- `Company ID` - Unique company identifier
- `Company Name` - Company name
- `Direct Group` - Primary grouping category
- `Group` - Secondary grouping category
- Additional transaction details as needed

## Algorithms Used

### Fuzzy Matching (RapidFuzz)
- **Method:** Token Sort Ratio
- **Threshold:** 90% similarity
- Effective for finding similar ledgers despite minor spelling variations
- Slower but more flexible for large variations in naming

### Levenshtein Distance
- **Method:** Edit distance with blocking optimization
- **Threshold:** 82% similarity
- Uses blocking keys (first 4 characters) to reduce comparison pairs
- Better performance for large datasets
- Effective for typos and character transpositions

## Notes

- Both fuzzy matching approaches (Ledger_analysis_3.ipynb and Ledger_analysis_4.ipynb) can be used depending on accuracy vs. performance requirements
- Low-frequency ledgers (< 5 transactions) are marked as "OTHER" for better analysis
- All Excel output cleans illegal control characters for compatibility
- Sheet names are truncated to 31 characters (Excel limitation) with numeric suffixes for duplicates

