# Interpretable Multi-stage Attention Network to Predict Cancer Subtype and Molecular Status of Microsatellite Instability, TP53 mutation and Tumor Mutational Burden of Endometrial and Colorectal Cancer

## Associated Publications
(Submitted) Wang et al. (2024) Interpretable Multi-stage Attention Network to Predict Cancer Subtype and Molecular Status of Microsatellite Instability, TP53 mutation and Tumor Mutational Burden of Endometrial and Colorectal Cancer

## Datasets
### Endometrial Cancer
A tissue microarray (TMA) cohort of patients' paraffin-embedded tissues was retrospectively retrieved from the Department of Pathology at Tri-Service General Hospital, Taipei, Taiwan and the National Defense Medical Center, Taipei, Taiwan. The TMA cohort contained 242 EC tissue cores with various morphological subtypes, including endometrioid carcinoma G1 (n=143), G2 (n=55), G3 (n=14), serous carcinoma (SC, n=11) and clear cell carcinoma (CC, n=19). The EC TMA tasks distribution, including cancer subtype [aggressive (n=44); non-aggressive (n=198)], TP53 prediction [mutation (n=115); wild type (n=127)], MLH1 prediction [loss (n=101); intact (138); NA: MSI data not available (n=3)], MSH2 prediction [loss (n=87); intact (n=151); NA: MSI data not available (n=4)], MSH6 prediction [loss (7); intact (n=182); NA: MSI data not available (n=3)] and PMS2 prediction [loss (120); intact (n=119); NA: MSI data not available (n=3)]. All reasonable requests for academic use of the independent EC TMAs dataset can be addressed to the corresponding authors.

### Colorectal Cancer
Secondly, we collected 1,945 H\&E-stained pathological slides from 594 colon cancer patients from 25 tissue source sites, including Mucinous Adenocarcinoma of the Colon and Rectum (n=61), Colon Adenocarcinoma (n=378) and Rectal Adenocarcinoma (n=155), were collected from TCGA. These images were extracted from 594 patients diagnosed with CRC, covering individuals aged 31 to 90 and representing over four different races. All TMB status determined by NGS [TMB-H: TMB $\geq 10$ (n=83); TMB-low: TMB $<10$ (n=451); NA: TMB data not available (n=60)]. Where all MSI status determined by NGS [MSI-High: MANTIS score $\geq 0.6$ (n=67); MSI-low: MANTIS score $< 0.6$ (n=490); NA: MSI data not available (n=37)]. 


## Experiment Setup

#### Requirements
- Ubuntu 18.04
- RAM >= 64 GB
- GPU Memory >= 12 GB
- GPU driver version >= 525.125.06
- CUDA version >= 12.0

Import the IMAN virtual environment in conda
```
conda env create --file=iman.yaml
```
then enter the environment:
```
conda activate iman
```

#### Download
The source code file, configuration file, and models (./IMAN/run/.../checkpoint.pth) can be downloaded from the [zip](https://drive.google.com/file/d/19rGkYAmGu5_-x5h8PMnXK1mqgEqxINak/view?usp=sharing) file. (For reviewers, the password of the zip file is provided in the "Code Availability" section of the associated manuscript.)

## Steps

#### 1. Bag-wise foreground extraction

Place the Whole slide images in ./WSI/TCGA
```
./WSI/TCGA/
├── slide_1.svs
├── slide_2.svs
│        ⋮
└── slide_n.svs
  
```

Execute the extraction code in a terminal as follows:
```
cd pre
python run_preprocess.py
```

The foreground bag files are saved in desired directory as arranged as follows.
```
./csv/Target_csv_Folder/
├── slide_1.csv
├── slide_2.csv
│        ⋮
└── slide_n.csv
  
```
Ensure the csv files contain the following example structure:

| bag_id | x | y |
|--|--|--|
| 6 | 12288 |0 |
| 30 |61440 |0 |
|....|...|
| 2385 |51200 |81920 |
  
#### 2. Divide training, validation and testing set

The format of the label file can refer to `IMAN/csv/label_example/sheet/train.csv` and `IMAN/csv/label_example/sheet/test.csv`:
| File Name | Sample Type |
|--|--|
| slide_1.svs | pos |
| slide_2.svs |neg  |
|....|...|
| slide_n.svs |neg |

Execute the code in a terminal as follows:
```
python pro_train.py
python pro_test.py
```

Ensure the output files (split folder) followed the directory structure below.

```
./csv/label_example/
    ├──sheet
        ├── train.csv (label)
        ├── test.csv (label)
        └── 91 (output files)
             └── 0
                 └── case_train.csv
                 └── case_val.csv
                 └── case_test.csv
    └──split (output files)
        └── 91
             └── 0
                 └── case_train.csv
                 └── case_val.csv
                 └── case_test.csv
    
```


#### 3. Training and inference step

#### Train IMAN
Adjust the `main.yaml` to set parameters before start training.
```
python main.py
```


#### Inference IMAN
```
python test.py
```



#### 4. Interpretability Analysis
For generating attention heatmaps, adjust the corresponding parameters on `heatmap.py` and run the code as follows.
```
python heatmap.py
```


## License
This Python source code is released under a creative commons license, which allows for personal and research use only. For a commercial license please contact Prof. Ching-Wei Wang. You can view a license summary here:  
http://creativecommons.org/licenses/by-nc/4.0/


## Contact
Prof. Ching-Wei Wang  
  
cweiwang@mail.ntust.edu.tw; cwwang1979@gmail.com  
  
National Taiwan University of Science and Technology

