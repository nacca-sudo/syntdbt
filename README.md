# BreasTomo-Synth Dataset: Quick Reference Guide
 
Code for the paper Synthetic Digital Breast Tomosynthesis Dataset with Tumor Segmentation Masks
## 🗂️ Visual Structure
 
```
ROI.csv ........... Main exam info (174 rows)
Volume.csv ........... Main exam info (30 rows)
```
 
---
 BreasTomo-Synth Dataset (400 MB)
│
├─ 📁 dbt_001 ...................... Examination 1
│  ├─ 📁 img_dbt_001
│  │  └─ 📄 BTS_VOL_001.tif ........ Multi-slice TIFF (~7 MB)
│  │                               8-bit grayscale, ~38 slices
│  │
│  └─ 📁 mask_dbt_001
│     └─ 📄 MASK_BTS_VOL_001.tif ........ Multi-slice TIFF (~7 MB)
│                                 Binary (0=background, 1=lesion)
│
├─ 📁 dbt_002 ...................... Examination 2
│  ├─ 📁 img_dbt_002
│  │  └─ 📄 BTS_VOL_002.tif
│  └─ 📁 mask_dbt_002
│     └─ 📄 MASK_BTS_VOL_002.tif
│
├─ 📁 dbt_003 through dbt_029 ...... Examinations 3-29 (same structure)
│
├─ 📁 dbt_030 ...................... Examination 30
│  ├─ 📁 img_dbt_030
│  │  └─ 📄 BTS_VOL_030.tif
│  └─ 📁 mask_dbt_030
│     └─ 📄 MASK_BTS_VOL_030.tif
│
└─ 📁 metadata ..................... Metadata folder
   ├─ 📊 dataset_metadata
```

## 📊 Dataset Content Summary
 
| Metric | Value |
|--------|-------|
| **Total Examinations** | 30 |
| **Total Image Volumes** | 30 |
| **Total Segmentation Masks** | 30 |
| **Additional ROI Patches** | 174 |
| **Density Categories** | 4 (Fatty, Scattered, Heterogeneous, Dense) |
| **Total TIFF Files** | 60 (30 images + 30 masks) |
| **Metadata Records** | 90+ (across 3 CSV files) |
| **Total Disk Space** | ~400 MB |
 
---
 
## 🔗 File Naming Pattern
 
### Three-Digit Zero-Padded Numbers
 
```
Exam Number | Folder Name        | Image File          | Mask File
------------|-------------------|---------------------|--------------------
1           | dbt_001            | BTS_VOL_001.tif     | MASK_BTS_VOL_001.tif
5           | dbt_005            | BTS_VOL_005.tif     | MASK_BTS_VOL_005.tif
15          | dbt_015            | BTS_VOL_015.tif     | MASK_BTS_VOL_015.tif
30          | dbt_030            | BTS_VOL_030.tif     | MASK_BTS_VOL_030.tif
```
 
**Key Rule**: Always use 3-digit format with leading zeros (dbt_001, NOT dbt_1)
 
---
 
## 📐 Image Specifications
 
### Image TIFF Files (img_dbt_0XX/BTS_VOL_0XX.tif)
 
```
Format:           Multi-page TIFF
Data Type:        Unsigned 8-bit integer 
Pixel Values:     X-ray intensity (higher = denser tissue)
Dimensions:       ~1130 × ~477 pixels per slice
Slices per File:  ~38 consecutive 2D slices
Total Size:       ~400 MB per examination
Voxel Spacing:    0.085 mm (isotropic)
```
 
### Mask TIFF Files (mask_dbt_0XX/BTS_VOL_0XX.tif)
 
```
Format:           Multi-page TIFF
Data Type:        Unsigned 8-bit integer (0 or 1)
Pixel Values:     Binary segmentation (0=normal, 1=lesion)
Dimensions:       Exactly matches corresponding image
Slices per File:  Exactly matches corresponding image
Total Size:       50-100 MB per examination
Annotation:       Manually delineated by expert operators
```
 
---
 
## 📋 Metadata CSV Files
 
### File 1: dataset_metadata.csv
**Purpose**: Main examination information
**Rows**: 30 (one per examination)
**Key Fields**:
- exam_id, density_category, breast_volume, compressed_thickness_mm
- voxel_spacing_mm, slice_thickness_mm, num_slices
- image_height, image_width, has_lesion
 
---
 
## 🏥 Breast Density Distribution
 
```
ACR Category              Count    Percentage
─────────────────────────────────────────────
Fatty                    6-8      20-27%
Scattered               8-10      27-33%
Heterogeneously Dense   8-10      27-33%
Dense                   4-6       13-20%
─────────────────────────────────────────────
TOTAL                    30       100%
```
 
---
 
## 💾 Disk Space Breakdown
 
```
Component                 Size        Percentage
──────────────────────────────────────────────────
30 Image TIFF Files      ~7 MB      80-90%
30 Mask TIFF Files        ~7 MB       7-15%
CSV Metadata Files        ~2 MB       <0.1%
Documentation              ~1 MB       <0.1%
──────────────────────────────────────────────────
TOTAL                    400 MB      100%
 
Per Examination:
  Image file              400-600 MB
  Mask file                50-100 MB
  Total                   450-700 MB
```
 
---
 
## 🔍 File Path Construction
 
### Python Path Building
 
```python
# Configuration
dataset_root = 'path/to/BreasTomo-Synth'
exam_number = 5
exam_id = f'dbt_{exam_number:03d}'      # Creates 'dbt_005'
 
# Image path
image_path = f'{dataset_root}/{exam_id}/img_{exam_id}/BTS_VOL_{exam_number:03d}.tif'
# Result: .../dbt_005/img_dbt_005/BTS_VOL_005.tif
 
# Mask path
mask_path = f'{dataset_root}/{exam_id}/mask_{exam_id}/BTS_VOL_{exam_number:03d}.tif'
# Result: .../dbt_005/mask_dbt_005/BTS_VOL_005.tif
 
# Metadata path
metadata_path = f'{dataset_root}/metadata/dataset_metadata.csv'
```
 
### Bash Command Examples
 
```bash
# List all exam folders
ls -d dbt_*/
 
# Count TIFF files
find . -name "*.tif" | wc -l
 
# Check folder structure
tree -L 2 dbt_001/
 
# Total dataset size
du -sh ./
 
# List files in specific exam
ls -lh dbt_001/img_dbt_001/
ls -lh dbt_001/mask_dbt_001/
```
 
## 🔗 Important Links
 
- **VICTRE Platform**: https://didsr.github.io/VICTRE_PIPELINE/
- **Python tifffile**: https://pypi.org/project/tifffile/
- **PyTorch Docs**: https://pytorch.org/docs/
- **ACR BI-RADS Lexicon**: https://www.acr.org/
- **Contact**: mail@hi.com
---
 
**Version**: 1.1  
**Last Updated**: July 2026  
**Status**: Public Release
