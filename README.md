# PhishIntention
<div align="center">

![Dialogues](https://img.shields.io/badge/Proctected\_Brands\_Size-277-green?style=flat-square)

</div>
<p align="center">
  <a href="https://www.usenix.org/conference/usenixsecurity22/presentation/liu-ruofan">Paper</a> •
  <a href="https://sites.google.com/view/phishintention](https://github.com/lindsey98/PhishIntention">Github</a> •
</p>

## PhishIntention
- This is the unofficial implementation of "Inferring Phishing Intention via Webpage Appearance and Dynamics: A Deep Vision-Based Approach"USENIX'22, with White-Box and Black-Box attacks on the model

## Framework
<img src="big_pic/Screenshot 2021-08-13 at 9.15.56 PM.png" style="width:2000px;height:350px"/>

```Input```: a screenshot, ```Output```: Phish/Benign, Phishing target
- Step 1: Enter <b>Abstract Layout detector</b>, get predicted elements

- Step 2: Enter <b>Siamese Logo Comparison</b>
    - If Siamese report no target, ```Return  Benign, None```
    - Else Siamese report a target, Enter step 3 <b>CRP classifier</b>
       
- Step 3: <b>CRP classifier</b>
   - If <b>CRP classifier</b> reports its a CRP page, go to step 5 <b>Return</b>
   - ElIf not a CRP page and havent execute <b>CRP Locator</b> before, go to step 4: <b>CRP Locator</b>
   - Else not a CRP page but have done <b>CRP Locator</b> before, ```Return Benign, None``` 

- Step 4: <b>CRP Locator</b>
   - Find login/signup links and click, if reach a CRP page at the end, go back to step 1 <b>Abstract Layout detector</b> with an updated URL and screenshot
   - Else cannot reach a CRP page, ```Return Benign, None``` 
   
- Step 5: 
    - If reach a CRP + Siamese report target: ```Return Phish, Phishing target``` 
    - Else ```Return Benign, None``` 

## Project structure
```
|_ configs: Configuration files for the object detection models and the gloal configurations
|_ modules: Inference code for layout detector, CRP classifier, CRP locator, and OCR-aided siamese model
|_ models: the model weights and reference list
|_ ocr_lib: external code for the OCR encoder
|_ utils
|_ configs.py: load configuration files
|_ phishintention.py: main script
```

## Instructions
Requirements: 
- Anaconda installed, please refer to the official installation guide: https://docs.anaconda.com/free/anaconda/install/index.html
- CUDA >= 11
  
1. Create a local clone of PhishIntention
```bash
git clone https://github.com/lindsey98/PhishIntention.git
cd PhishIntention
```

2. Setup.
In this step, we would be installing the core dependencies of PhishIntention such as pytorch, and detectron2. 
In addition, we would also download the model checkpoints and brand reference list.
This step may take some time.
```bash
chmod +x setup.sh
export ENV_NAME="phishintention"
./setup.sh
```

3. 
```bash
conda activate phishintention
```

4. Run
```bash
python phishintention.py --folder <folder you want to test e.g. datasets/test_sites> --output_txt <where you want to save the results e.g. test.txt>
```
The testing folder should be in the structure of:

```
test_site_1
|__ info.txt (Write the URL)
|__ shot.png (Save the screenshot)
|__ html.txt (HTML source code, optional)
test_site_2
|__ info.txt (Write the URL)
|__ shot.png (Save the screenshot)
|__ html.txt (HTML source code, optional)
......
```

5. White-Box Attacks
```bash
python WhiteBox_Attack.py  
```

6. Black-Box Attacks
```bash
python BlackBox_Attack.py  
```

## Miscellaneous
- Implementation of several phishing detection and identification baselines, see [here](https://github.com/lindsey98/PhishingBaseline)

## Citation

```bibtex
@inproceedings{liu2022inferring,
  title={Inferring Phishing Intention via Webpage Appearance and Dynamics: A Deep Vision Based Approach},
  author={Liu, Ruofan and Lin, Yun and Yang, Xianglin and Ng, Siang Hwee and Divakaran, Dinil Mon and Dong, Jin Song},
  booktitle={30th $\{$USENIX$\}$ Security Symposium ($\{$USENIX$\}$ Security 21)},
  year={2022}
}
```

