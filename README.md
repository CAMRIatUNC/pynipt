[![DOI](https://zenodo.org/badge/171055555.svg)](https://zenodo.org/badge/latestdoi/171055555)
# PyNIPT (Python NeuroImage Pipeline Tool)
#### Version: 0.2

### Description:
- The PyNIPT module is a pipeline framework for neuroimaging data analysis that offers a convenient, and yet powerful data processing management features under Jupyter Notebook environment. The module is designed to take input from the BIDS dataset and organize the derivates into the block of steps directories instead of using prefix or suffix to modify the filename. Therefore it preserves the original filename during the data processing while the derivates of each pipeline node organized into a single directory. 
- The key features of this module are 
    1. Enabling to execute the command-line interface or python script without input file path specification. The selection of the set of files could be performed via selecting node block and using the regex (regular expression) pattern of the file name.
    2. Continuity of data processing in a single Jupyter notebook session. The module executes the command through background scheduler, so that the Jupyter notebook does not block during data processing.
    3. Providing the API to simplify the development of analysis tools and processing pipeline. The easy-to-use debugging tool also maximizes the convenience of development. 
    
- ***Dependency:***
    - pandas >= 1.0.0 
    - tqdm >= 4.40.0
    - psutil >= 5.5.0
    - paralexe >= 0.1.0
    - shleeh >= 0.0.6

- ***Compatibility:*** 
    - Python > 3.7.5 and <3.8
    - Brain imaging data structure (http://bids.neuroimaging.io)
    - Jupyter notebook environment (https://jupyter.org)

- ***ChangeLog:***
    - v0.2.1 (5/24/2020)    - critical bug patch
    - v0.2.0 (5/24/2020)    - user interface for debugging
    
### Installation
- from PyPI
```js
$ pip install pynipt
```

- from Github repository
```js
$ pip install git+https://github.com/pynipt/pynipt
```

### Example Project Data Structure
```js
Project_Root/
├── JupyterNotes/
│   └── fMRI_Data_Preprocessing.ipynb
├── Data/
│   ├── dataset_description.json
│   ├── README
│   ├── sub-01/
│   │   ├── anat/
│   │   │   ├── sub-01_T2w.json
│   │   │   └── sub-01_T2w.nii.gz
│   │   ├── fmap/
│   │   │   ├── sub-01_fieldmap.json
│   │   │   ├── sub-01_fieldmap.nii.gz
│   │   │   └── sub-01_magnitude.nii.gz
│   │   └── func/
│   │       ├── sub-01_task-rest_bold.json
│   │       ├── sub-01_task-rest_bold.nii.gz
│   │       ├── sub-01_task-active_bold.json
│   │       └── sub-01_task-active_bold.nii.gz
│   └── sub-02/
│       ├── anat/
│       │   ├── sub-02_T2w.json
│       │   └── sub-02_T2w.nii.gz
│       ├── fmap/
│       │   ├── sub-02_fieldmap.json
│       │   ├── sub-02_fieldmap.nii.gz
│       │   └── sub-02_magnitude.nii.gz
│       └── func/
│           ├── sub-02_task-rest_bold.json
│           ├── sub-02_task-rest_bold.nii.gz
│           ├── sub-02_task-active_bold.json
│           └── sub-02_task-active_bold.nii.gz
├── Mask/
│   ├── 02A_BrainMasks-func/
│   │   ├── sub-01/
│   │   │   ├── sub-01_task-rest_bold.nii.gz
│   │   │   └── sub-01_task-active_bold_mask.nii.gz
│   │   └── sub-02/
│   │       ├── sub-02_task-rest_bold.nii.gz
│   │       └── sub-02_task-active_bold_mask.nii.gz
│   └── 02B_BrainMasks-anat/
│       ├── sub-01/
│       │   ├── sub-01_T2w.nii.gz
│       │   └── sub-01_T2w_mask.nii.gz
│       └── sub-02/
│           ├── sub-02_T2w.nii.gz
│           └── sub-02_T2w_mask.nii.gz
├── Processing/
│   └── MyPipeline/
│       ├── 01A_ProcessingStep1A-func/
│       │   ├── sub-01/
│       │   │   ├── sub-01_task-rest_bold.nii.gz
│       │   │   └── sub-01_task-active_bold.nii.gz
│       │   ├── sub-02/
│       │   │   ├── sub-02_task-rest_bold.nii.gz
│       │   │   └── sub-02_task-active_bold.nii.gz
│       └── 01B_ProcessingStep1B-func/
│           ├── sub-01/
│           │   ├── sub-01_task-rest_bold.nii.gz
│           │   └── sub-01_task-active_bold.nii.gz
│           └── sub-02/
│               ├── sub-02_task-rest_bold.nii.gz
│               └── sub-02_task-active_bold.nii.gz
├── Results/
│   └── MyPipeline/
│       └── 030_2ndLevelStatistic-func/
│           ├── TTest.nii.gz
│           └── TTest_report.html
├── Temp/
├── Logs/
│   ├── DEBUG.log
│   ├── STDERR.log
│   └── STDOUT.log
└── Templates/
    └── BrainTemplate.nii.gz
```
#### A Project folder is composed of 6 data components as below
- **Data**: naive BIDS dataset (This is the only required folder when you start)
- **Mask**: the folder to store outputs for single subject-level image segmentation results (such as brain mask), which may manual refinement could be required.
- **Processing**: the folder to store intermediate files that generated by this module, it could be used as input for later processing nodes.
- **Results**: the folder to store the report files that does not preserve original data structure, such as group-level analysis.
- **Temp**: the folder to store the intermediate files that can be disposed without worry. (which means not important to keep for the further process)
- **Log**: the folder to keep log files. All logging message including debugging, standard output, and error messages from sub-processors will be recorded here.
#### Optional data components can be used as below (up to you)
- **JupyterNotes**: the folder to store Jupyter notebook. Source code, Documentation and Visualizing outputs.
- **Templates**: to store anatomical brain image template, reference images, group level brain masks, and labelled brain atlas.

### Getting started
- Start Pipeline from scratch
```python
>> import pynipt as pn
>> pipe = pn.Pipeline(<ProjectRoot Path>)
** Dataset summary

Path of Dataset: /absolute/path/to/<ProjectRoot Path>
Name of Dataset: <ProjectRoot Path>
Selected DataClass: Data

Subject(s): ['sub-01', 'sub-02']
Datatype(s): ['anat', 'fmap', 'func']


List of installed pipeline packages:
>> pipe.set_scratch_package('MyPipeline')
The scratch package [MyPipeline] is initiated.
```

- Execute command 'mycommand' for the all file in Datatype 'func' and output files to Processing/01A_ProcessingStep1A-func.
```python
>> itb = pipe.get_builder(n_threads=1)   # in case mycommand take huge computing resources
>> itb.init_step(title='ProcessingStep1A', suffix='func',
>>               idx=1, subcode='A', mode='processing', type='cmd')
>> itb.set_input(label='input', input_path='func')
>> itb.set_var(label='param', value=10)
>> itb.set_output(label='output')
>> itb.set_cmd('mycommand -i *[input] -o *[output] -o *[param]')
>> itb.set_output_checker('output')
>> itb.run()
```

- Execute command 'getmask' for the first file in Datatype '01A' to generate brain mask and output to Mask/02A_BrainMasks-func.
additionally copy the original data to output folder
```python
>> itb = pipe.get_builder(n_threads=4)  # multi threasing
>> itb.init_step(title='BrainMasks', suffix='func',
>>               idx=2, subcode='A', mode='masking', type='cmd')
>> itb.set_input(label='input', input_path='01A', idx=0,
>>               filter_dict=filter_dict)
>> itb.set_output(label='mask', suffix='_mask')
>> itb.set_output(label='copy')
>> itb.set_cmd('getmask *[input] *[mask]')
>> itb.set_cmd('cp *[input] *[copy]')
>> itb.set_output_checker('mask')
>> itb.run()
```

- Execute python function 'myfunction' for the all file in StepCode '01A' and output files to Processing/01B_ProcessingStep1B-func.
```python
>> def myfunction(input, output, param, stdout=None, stderr=None):
>>     import sys
>>     import numpy as np
>>     import nibabel as nib
>>     if stdout == None:
>>         stdout = sys.stdout
>>         stderr = sys.stderr
>>     try:
>>         stdout.write(f'Running MyFunction for input: {input}\n')
>>         img = nib.load(input)
>>         img_data = np.asarray(img.dataobj)
>>         result_data = img_data * param
>>         stdout.write(f'Multiply image by {param}\n')
>>         nii = nib.Nifti1Image(result_data, affine=img._affine, header=img._header)
>>         stdout.write(f'Save to {output}..\n')
>>         nii.to_filename(output)
>>         stdout.write('Done\n')
>>     except:
>>         stderr.write('[ERROR] Failed!\n')
>>         return 1
>>     return 0
>>
>> itb = pipe.get_builder()
>> itb.init_step(title='ProcessingStep1B', suffix='func',
>>               idx=1, subcode='B', mode='processing', type='python')
>> itb.set_input(label='input', input_path='01A')
>> itb.set_var(label='param', value=10)
>> itb.set_output(label='output')
>> itb.set_func(myfunction)
>> itb.set_output_checker('output')
>> itb.run()
```

- Simple example of 2nd level stats (TTest), 'rest' vs 'active'
```python
>> def myttestfuncion(group_a, group_b, output, stdout=None, stderr=None):
>>    import sys
>>    if stdout is None:
>>       stdout = sys.stdout
>>    if stderr is None:
>>        stderr = sys.stderr
>>    
>>    import nibabel as nib
>>    import numpy as np
>>    import scipy.stats as stats
>>    affine = None
>>    try:
>>        groups = dict()
>>        for group_id, subj_list in dict(group_a=group_a, group_b=group_b).items():
>>            stack = []
>>            for i, img_path in enumerate(input):
>>                stdout.write(f'{img_path} is loaded as group {group_id}')
>>                img = nib.load(img_path)
>>                if i == 0:
>>                    affine = img.affine
>>                stack.append(np.asarray(img._dataobj))
>>            groups[group_id] = np.concatenate(stack, axis=-1)
>>            stdout.write(f'{group_id} has been stacked.')
>>        t, p = stats.ttest_ind(groups['group_a'], groups['group_b'], axis=-1)
>>        imgobj = np.concatenate([t[..., np.newaxis], p[..., np.newaxis]], axis=-1)
>>        ttest_result = nib.Nifti1Image(imgobj, affine)
>>        ttest_result.to_filename(output)
>>        stdout.write('{} is created'.format(output))
>>        open (f'{output}_report.html', 'w') as f:
>>            f.write('<html>Hello world.</html>\n')
>>    except Exception as e:
>>        stderr.write(str(e))
>>        return 1
>>    return 0
>>
>> itb = pipe.get_builder()
>> itb.init_step('2ndLevelStatistic', suffix='func', idx=3, subcode=0, 
>>               mode='reporting', type='python') # for reporting, as a default, output is directory without extension
>> itb.set_input(label='group_a', input_path='01B', group_input=True, 
>>               filter_dict=dict(regex=r'sub_\d{2}_task-rest.*'), join_modifier=False) # if this is False, input will return 
>>                                                                                     # 'list obj' so can run loop within python function
>> itb.set_input(label='group_b', input_path='01B', group_input=True,
>>               filter_dict=dict(regex=r'sub_\d{2}_task-active.*'), join_modifier=False)
>> itb.set_output(label='output', 
>>                modifier='TTest', ext='nii.gz') # for peers to one output
>> itb.set_func(myttestfunc)
>> itb.set_output_checker(label='output')  # this will check if TTest.nii.gz is generated but not html file
>> itb.run()
``` 

- Check progression with using progressbar
```python
>> pipe.check_progression()
MyPipeline 50%|████████████████                  | 1/2
```

- Access a image file absolute path in dataset '01A'
```python
>> pipe.get_dset('01A').df  # get_dset method will return dataset object
...will print out the data structure...

>> file_1 = pipe.get_dset('01A')[0].Abspath  # get absolute path of first indexed file 
```


- To get more detail information, please check below links
    - [Notebook Examples](https://github.com/PyNIPT/pynipt/tree/master/examples) *This link is not up-to-date, but will give you some insight how this module works*
    - [Example plugin](https://github.com/PyNIPT/pynipt-plugins/tree/master/uncch_camri)


#### Regular expression for data filtering
- Regex patterns using in this module
    - This module use regular expression to search specific filename without extension, 
    so the file extension must be provided as separate filter key.
- Filter key 
    - Dataclass specific keys
        - 'Data': dataset path  (idx:0): subjects, datatypes
        - 'Processing': working path  (idx:1): pipelines, steps
        - 'Results': results path  (idx:2): pipelines, reports
        - 'Mask': masking path  (idx:3): subjects, datatypes
        - 'Temp': temporary (idx:4): pipelines, steps
    - File specific keys
        - regex: regex pattern for filename
        - ext: file extension
        
- Output filename specification
    - Using prefix, suffix, and/or modifier will result in the change of output filename. 
    - The modifier is the key-value paired python dictionary object. the value in key will be searched and will be replaced to the string in value.
    - Or in case of reporting purpose (which the group_input=1), single string can be use as output filename

- Output checker
    - The output checker required to validate if the result file is generated.
    - If the output filename is same as the one you specified in output, you only need to input the label of your output.
    - However, some tools generate multiple files which result in modifying filename.
    - In this case, you can specify the prefix and suffix here to let the processor knows what file you want to check to validate the success of process.

### The StepCode to access data
- Step code is designed to enhance data accessibility of specific processing node without knowing the data structure.
- In PyNIPT, each processing step required to assign unique StepCode composed of 3 characters. (e.g. '03E')
    - The first two integers are to identify 'the level of process'. Total 100 levels (00 to 99) are available.
    - The last one character is to identify 'the sub-step of each level'. Total 27 sub-step can be specified (0 or A-Z)
    - The sub-step can be used 
- Example folder name of one processing node: '01E_MotionCorrection-func'
    - The '01E' is StepCode
    - The 'MotionCorrection' is the title of processing node
    - The 'func' is the suffix for distinguish the node if the same processing node is used multiple time. (the same folder is not allowed to use multiple time, so using suffix is crucial.)

### Tutorials
*The tutorial does not ready yet, will be provided soon*
    
#### License

PyNIPT is licensed under the term of the GNU GENERAL PUBLIC LICENSE Version 3

#### Authors
- SungHo Lee (shlee@unc.edu): core developer
- Woomi Ban (banwoomi@unc.edu): debugging and testing
- Yen-Yu Ian Shih (shihy@neurology.unc.edu): technical and academical advisory on this project (as well as funding)

#### Contributors
If you interest in contributing this project, please contact shlee@unc.edu.

#### Citing PyNIPT
Lee, SungHo, Ban, Woomi, & Shih, Yen-Yu Ian. (2020, May 25). PyNIPT/pynipt: PyNIPT v0.2.1 (Version 0.2.1). Zenodo. http://doi.org/10.5281/zenodo.3842192

```js
@software{lee_sungho_2020_3842192,
  author       = {Lee, SungHo and
                  Ban, Woomi and
                  Shih, Yen-Yu Ian},
  title        = {PyNIPT/pynipt: PyNIPT v0.2.1},
  month        = may,
  year         = 2020,
  publisher    = {Zenodo},
  version      = {0.2.1},
  doi          = {10.5281/zenodo.3842192},
  url          = {https://doi.org/10.5281/zenodo.3842192}
}
```