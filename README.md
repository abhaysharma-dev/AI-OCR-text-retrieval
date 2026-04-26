AI OCR Receipt Information Extraction System

Code Repository : https://github.com/abhaysharma-dev/AI-OCR-text-retrieval

1. Approach

The objective of this project was to build an OCR-based pipeline capable of extracting structured information from semi-structured receipt images containing multiple layouts, noisy scans, varying fonts, skewed images, and real-world inconsistencies.

-

Step 1: Image Input

The provided dataset consisted of receipt images with different formats and varying image quality levels. These images were loaded from the dataset folder for batch processing.

-

Step 2: Image Preprocessing

Initially, preprocessing techniques such as grayscale conversion, thresholding, and denoising were tested using OpenCV. However, during experimentation it was observed that applying preprocessing on already clean receipts reduced OCR performance by distorting text.

Therefore, I decided to continure without preprocessing.

-

Step 3: Text Detection and Recognition

EasyOCR was used for text detection and recognition.

The OCR engine extracted text tokens from each receipt image and generated raw textual outputs which were further processed for structured extraction.

-

Step 4: Key Information Extraction

Rule-based heuristics and regular expressions were used to extract:

Store Name
Transaction Date
Purchased Items
Item Prices
Total Amount

Since OCR outputs were inconsistent across receipts, multiple heuristics were implemented:

Store names extracted from top receipt text
Date extracted using regex patterns
Item prices extracted by identifying decimal values
Total amount extracted using keywords such as TOTAL, SUBTOTAL, etc.

- 

Step 5: JSON Structuring

For each receipt, extracted information was stored in structured JSON format.

Example:

{
  "store_name": "WALMART",
  "date": "10/20/07",
  "items": [
    {
      "name": "TOOTHBRUSH",
      "price": "0.96"
    },
    {
      "name": "WOMEN SLIPPE",
      "price": "9.86"
    }
  ],
  "total_amount": 18.75
}

-

2. Expense Summary Output

The expense summary was generated using the extracted JSON files.

Results:
Total Spend: ₹71,312.81
Total Transactions Processed: 117

The summary was calculated by aggregating all valid total_amount values from the generated receipt JSON outputs.

-

3. Tools Used
Python
EasyOCR
OpenCV
Regular Expressions (Regex)
JSON
Pandas

-

4. Challenges Faced

i. OCR Token Fragmentation

In many receipts, item names and prices were split into separate tokens.

Example:

BANANAS
0.20

or

TOOTHBRUSH 003500055500 0.96

This required multiple extraction heuristics.

ii. Noisy OCR Outputs

Some receipts contained:

Blurry text
Skewed scans
Poor lighting
Background noise

These issues reduced OCR accuracy.

iii. Metadata Misclassification

Store IDs, transaction IDs, and phone numbers were sometimes incorrectly identified as product names.

iv. Different Receipt Layouts

Receipts followed different structures which made rule-based extraction challenging.

-

5. Improvements / Future Work
Implement confidence scoring using OCR confidence values
Fine-tune OCR models for receipt-specific extraction
Use LayoutLM / Named Entity Recognition models for better structured extraction
Improve handling of highly noisy receipts
Build scalable batch processing pipeline for large datasets