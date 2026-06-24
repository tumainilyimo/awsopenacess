# ImageSafari Crop Image Collection

The ImageSafari Crop Image Collection is an open, curated set of field
photographs representing 18 staple food and legume crops. Images were collected
at research stations and farms across eight African countries through 15
research centres under the ImageSafari initiative.

The published collection contains **6,081,555 cleaned images** selected from a
working corpus of 7,352,184 images. The collection is intended for training and
evaluating computer-vision models, crop identification, phenotyping research,
agricultural monitoring, domain-generalization studies, and image-data curation
research.

## Dataset summary

| Attribute | Value |
|---|---:|
| Published images | 6,081,555 |
| Crops | 18 |
| Countries represented in the source corpus | 8 |
| Contributing research centres | 15 |
| Image formats | JPEG, PNG, WebP |
| License | CC BY-SA 4.0 |

## Crops and image counts

| Crop | Images | Share |
|---|---:|---:|
| Potato | 1,689,917 | 27.8% |
| Sorghum | 730,682 | 12.0% |
| Pigeon pea | 698,011 | 11.5% |
| Finger millet | 593,461 | 9.8% |
| Cowpea | 519,759 | 8.5% |
| Sweet potato | 462,894 | 7.6% |
| Groundnut | 411,948 | 6.8% |
| Banana | 334,732 | 5.5% |
| Pearl millet | 211,390 | 3.5% |
| Yam | 117,494 | 1.9% |
| Lentil | 78,218 | 1.3% |
| Chickpea | 73,856 | 1.2% |
| Rice | 53,170 | 0.9% |
| Wheat | 37,023 | 0.6% |
| Maize | 23,348 | 0.4% |
| Common bean | 22,295 | 0.4% |
| Cassava | 12,120 | 0.2% |
| Soybean | 11,237 | 0.2% |
| **Total** | **6,081,555** | **100%** |

## Geographic and institutional coverage

The source corpus covers Kenya, Tanzania, Uganda, Malawi, Nigeria, Senegal,
Cote d'Ivoire, and Ghana. Contributions came through 15 research centres:

- africa-rice-ci
- africa-rice-ibadan
- africa-rice-sn
- ciat-arusha
- cimmyt-kiboko
- cimmyt-njoro
- cimmyt-senegal
- cip-nairobi
- csir-ghana
- cummyt-kibos
- iita-kampala
- iita-kano
- iita-nambala
- iita-osogbo
- qed-lilongwe

Kenya and Tanzania account for most images in the source corpus. This
concentration should be considered when designing train, validation, and test
splits or evaluating geographic generalization.

## Seasonal coverage

Images are tagged with one of four growing-season labels: fall, spring, summer,
or winter. Seasonal coverage varies substantially by crop.

| Crop | Fall | Spring | Summer | Winter | Total |
|---|---:|---:|---:|---:|---:|
| Potato | 324,357 | 783,180 | 2,449 | 579,931 | 1,689,917 |
| Sorghum | 316,497 | 128,225 | 31,440 | 254,520 | 730,682 |
| Pigeon pea | 385,810 | 0 | 8,276 | 303,925 | 698,011 |
| Finger millet | 293,474 | 54 | 10,243 | 289,690 | 593,461 |
| Cowpea | 164,564 | 73,320 | 9,720 | 272,155 | 519,759 |
| Sweet potato | 281,536 | 26,297 | 11,063 | 143,998 | 462,894 |
| Groundnut | 261,342 | 33,525 | 6,034 | 111,047 | 411,948 |
| Banana | 9,593 | 163,349 | 0 | 161,790 | 334,732 |
| Pearl millet | 150,794 | 36,995 | 5,548 | 18,053 | 211,390 |
| Yam | 0 | 82,069 | 0 | 35,425 | 117,494 |
| Lentil | 0 | 2,219 | 1,587 | 74,412 | 78,218 |
| Chickpea | 0 | 4,211 | 941 | 68,704 | 73,856 |
| Rice | 0 | 40,092 | 0 | 13,078 | 53,170 |
| Wheat | 942 | 31,650 | 0 | 4,431 | 37,023 |
| Maize | 3,345 | 5,189 | 1,986 | 12,828 | 23,348 |
| Common bean | 3,170 | 583 | 18,540 | 2 | 22,295 |
| Cassava | 2,095 | 3,455 | 0 | 6,570 | 12,120 |
| Soybean | 4,011 | 0 | 7,226 | 0 | 11,237 |

## Included data

The release contains cleaned still images in JPEG, PNG, and WebP formats.

The following materials are not included:

- Files rejected during curation because they were corrupted, exact
  duplicates, empty, or identified as off-type or wrong-crop images


## Data organization

Images are organized by crop at the top level of the S3 bucket. Object keys
retain available acquisition metadata, including country, season, capture
method, date, contributing centre, and file format.

Representative structure:

```text
s3://<S3-BUCKET-NAME>/<crop>/.../<year>:<country>:<crop>:<season>/.../<capture-method>/<capture-date>/<file>.<extension>
```

Representative object:

```text
s3://<S3-BUCKET-NAME>/maize/.../2025:ken:maize:winter/.../phone_approach/2025-09-19/<file>.jpg
```

The precise key structure can vary across contributing centres. Users should
read metadata defensively and treat unavailable values as `unknown`.

## Metadata

Metadata are derived from the object key and source inventory.

| Field | Description |
|---|---|
| `crop` | Crop depicted in the image |
| `country` | Country of capture, where available |
| `season` | Fall, spring, summer, or winter |
| `capture_method` | Image-acquisition approach, such as phone-based capture |
| `year` | Year of capture |
| `capture_date` | Capture date in `YYYY-MM-DD` form, where available |
| `centre` | Contributing research centre or station |
| `file_format` | JPEG, PNG, or WebP, inferred from the extension |

## Accessing the data

The collection is publicly readable. The commands below use anonymous (unsigned)
access, so no AWS account or credentials are required.

List the top-level crop prefixes:

```bash
aws s3 ls --no-sign-request --region <AWS-REGION> s3://<S3-BUCKET-NAME>/
```

List the objects for one crop:

```bash
aws s3 ls --no-sign-request --region <AWS-REGION> --recursive s3://<S3-BUCKET-NAME>/potato/
```

Download one crop:

```bash
aws s3 sync --no-sign-request --region <AWS-REGION> s3://<S3-BUCKET-NAME>/potato/ ./potato/
```

Copy one image:

```bash
aws s3 cp --no-sign-request --region <AWS-REGION> s3://<S3-BUCKET-NAME>/<OBJECT-KEY> .
```

Use anonymous access from Python:

```python
import boto3
from botocore import UNSIGNED
from botocore.config import Config

bucket = "<S3-BUCKET-NAME>"
region = "<AWS-REGION>"

s3 = boto3.client(
    "s3",
    region_name=region,
    config=Config(signature_version=UNSIGNED),
)

response = s3.list_objects_v2(
    Bucket=bucket,
    Prefix="potato/",
    MaxKeys=10,
)

for item in response.get("Contents", []):
    print(item["Key"], item["Size"])
```

## Curation

The published collection was produced from the 7,352,184-image working corpus
through the following process:

1. **Hard pre-filtering:** files that could not be decoded, exact duplicates
   identified by MD5 hash, and empty frames with grayscale pixel standard
   deviation below 5 were removed.
2. **Feature extraction:** surviving images were represented using
   1,024-dimensional DINOv3 ViT-L/16 embeddings.
3. **Outlier detection:** global candidates were identified using cosine
   k-nearest-neighbour distance with `k=10` and a median plus three median
   absolute deviations threshold. Local Outlier Factor was used where
   within-crop distributions were multi-modal.
4. **Region review:** UMAP projections and k-means groupings supported review
   of coherent regions containing off-type or wrong-crop images.
5. **Expert review:** crop-domain experts reviewed the cleaned subsets before
   publication.

Mild blur was retained because it can represent realistic field conditions and
may provide useful signal for robust model development.

## Known characteristics and limitations

- The collection is strongly imbalanced. Potato accounts for 27.8% of the
  images, while soybean and cassava each account for less than 0.3%.
- Geographic contributions are uneven, with Kenya and Tanzania dominating the
  source corpus.
- Visually similar crops and growth stages can make fine-grained
  classification difficult.


## Suggested research tasks

- Cross-centre and cross-country domain generalization
- Dataset quality assessment and scalable curation
- Self-supervised representation learning for African crop imagery
- Few-shot learning for crops with limited representation

## License

The dataset is released under the
[Creative Commons Attribution-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-sa/4.0/).

Users may share and adapt the material for any purpose, including commercial
use, provided that they give appropriate credit, link to the license, indicate
whether changes were made, and distribute adapted material under the same
license.

## Citation

Use the following citation:

> Alliance of Bioversity International and CIAT (2026). *ImageSafari Crop
> Image Collection*. Registry of Open Data on AWS.

Also include the date on which the data were accessed and identify the release
or S3 prefix used in the analysis.

## Contact

Questions, corrections, and dataset issues can be submitted through the
[ImageSafari repository issue tracker](https://github.com/CIAT-Artemis/imagesafari-crop-images/issues).

## Acknowledgements

ImageSafari is led by the Alliance of Bioversity International and CIAT, part
of CGIAR, with images contributed by participating research centres. AWS
hosting is supported through the AWS Open Data Sponsorship Program.
