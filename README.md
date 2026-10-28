# Consignment Tags: From Item List to Printed Tags

<img src="images/repo-qr.png" alt="QR code linking to the GitHub repository" width="180" align="right">

**Get the tool and the latest instructions:**
<https://github.com/scottkuma/consignment-tag-printer>

Scan the QR code with a phone camera to open the repository.

<br clear="all">

This describes how a list of items becomes print-ready price tags using
`consignment_tag_printer.html`.

## Files in this folder

| File | Purpose |
|------|---------|
| `consignment_tag_printer.html` | Self-contained tag generator. Runs locally in the browser; nothing is uploaded. |
| `peddlers_mall_locations.csv` | Source list for the Location dropdown (names are also embedded in the HTML). |
| `images/` | Example handwritten page and the repository QR code used in this README. |
| `CSV Files and Tags/` | Working folder for item-entry CSVs, Consignor Access exports and generated PDFs. |
| `CSV Files and Tags/Archive/` | Dated copies of past exports and tag PDFs. |

## Steps

### 1. Build the item-entry CSV

The goal of this step is a CSV file in the **`Standard_Item_Entry_Template.csv`** format.
There are several ways to get one. **Choose whichever suits the seller:**

- **Fill in the template by hand.** The seller can type the items straight into
  `Standard_Item_Entry_Template.csv` (in Excel, Numbers, Google Sheets or any text editor).
- **Export it from a third-party inventory program.** If the seller already tracks inventory in
  another app or spreadsheet, it can output the items in the `Standard_Item_Entry_Template.csv` format for them.
- **Convert handwritten notes with an AI** (steps below).

Handwritten notes like this one can be turned into the CSV automatically:

![Example of a handwritten inventory page: item description, size and price on each line](images/example-handwritten-page.jpg)

1. Photograph the handwritten inventory notes (one image per page).
2. Attach the photos and the template CSV file to the chat in Claude or another AI.
3. Use the AI to convert the photos into a combined CSV that follows the
   `Standard_Item_Entry_Template.csv` format
   (output: `Combined_Item_Entry_YYYYMMDD.csv`).

   **Example prompt** (attach the photos and the template CSV to the chat, in Claude or another AI):

   ```text
   I've attached photos of handwritten consignor inventory notes (Item Entry pages),
   plus Standard_Item_Entry_Template.csv.

   Please read every item on every photo and create one combined CSV that follows
   the exact column headers and order of the template.

   Rules:
   - One row per item line. Do not skip, merge or invent items.
   - Keep the item names as written, fixing only obvious spelling slips.
   - Quantity defaults to 1 if none is written.
   - Prices are numbers only (no $ sign), e.g. 12.50.
   - If something is unreadable, still include the row and put "CHECK" in the
     item name so I can find it.

   When done, give me the CSV as a downloadable file named
   Combined_Item_Entry_YYYYMMDD.csv (today's date), then list any rows
   you were unsure about, with the photo and line number.
   ```

   Tip: Some AIs allow you to create a skill that will do this automatically when you upload photos.

4. Skim the CSV and fix any misread names, quantities or prices.

### 2. Import into Consignor Access
1. Log in to Consignor Access (SimpleConsign) and make sure the **Store** at the top right is the correct location (e.g. Lebanon).
2. In the left-hand menu, click **Item Entry**.
3. Click **Import CSV** (next to **+ Add**) and choose `Combined_Item_Entry_YYYYMMDD.csv`.
4. Check that the imported rows appear under **Items To Be Entered** (Name, UPCs, Quantity, Price, Do Not Discount, Category) and fix any problems.
5. Click **Submit** to enter the items.
6. Confirm the items now appear under **Items** with **Status = Active** and have SKUs assigned.

### 3. Export the items list
The seller needs to export the list of items that were just entered.

1. In Consignor Access, click **Items** in the left-hand menu (**Vendor Items**).
2. Click the **date range** drop-down at the top of the screen, choose **Today**, then click **Submit**.
   This limits the list to the items just entered.
3. In the **Show all status** drop-down, select **ACTIVE**.
4. Use the check boxes to select the items to export (or the box in the header row to select them all).
5. Click the **CSV** button (with the download icon) on the right.
6. Save the downloaded file in `CSV Files and Tags/` as `Consignor_Access_-_ItemsYYYYMMDD.csv`.
   The tag printer needs these columns: **Item Name, SKU, Quantity, Status, Retail Price**.

### 4. Generate the front tags
1. Open `consignment_tag_printer.html` in a browser.
2. **1. CSV File:** click or drag in the exported items CSV (`Consignor_Access_-_ItemsYYYYMMDD.csv`).
3. **2. Tag Label:**
   - Choose the **Location** from the dropdown (top line of the tag).
   - Enter the **Code** (second line, e.g. `DP006`).
   - Options (defaults in parentheses):
     - Strip trailing "(MERCHANDISE)"-style text from names (on)
     - Only include Status = Active (on)
     - One tag per unit of Quantity (on)
     - Show dashed cut guides (off)
4. Check the preview, then click **Download PDF**
   (saved as `<Location>_tags.pdf`; rename to `<Location>_tags-YYYYMMDD.pdf` if you like).

Location, Code and the Back of Tag settings are remembered in the browser between runs.

### 5. Generate the back-side PDF (optional)
1. In **4. Back of Tag**, set the booth name, font, ink color and optional logo.
2. Leave **Rotate 180°** on if your printer's duplex flips on the long edge.
3. Click **Download Back-Side PDF**. It always has the same page count as the fronts.
   This only needs regenerating if the back design or page count changes.

### 6. Print and cut
- Paper: 8.5" x 11". Tags are 4.15" x 1.05", 2 across x 10 down (20 per page).
- Print at **100% / Actual size** (no "fit to page").
- Print the fronts, flip the stack, then print the backs (or use duplex).
- Cut along the grid and attach the tags.

### 7. Archive
Move the dated item CSV and tag PDF(s) into `CSV Files and Tags/Archive/`.

## Notes
- **Adding or changing locations:** edit the `<select id="locInput">` options in the HTML
  (and update `peddlers_mall_locations.csv` to match).
- **Saved settings** are stored in the browser's localStorage, so they are per browser and
  per file location. A new browser or a moved file starts from the defaults.
- Barcodes are Code128, generated from the SKU column.
