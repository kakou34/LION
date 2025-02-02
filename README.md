## <p align="center">LION: Latent Point Diffusion Models for 3D Shape Generation<br><br> NeurIPS 2022 </p>
<div align="center">
  <a href="https://www.cs.utoronto.ca/~xiaohui/" target="_blank">Xiaohui&nbsp;Zeng</a> &emsp; 
  <a href="http://latentspace.cc/" target="_blank">Arash&nbsp;Vahdat</a> &emsp; 
  <a href="https://www.fwilliams.info/" target="_blank">Francis&nbsp;Williams</a> &emsp; 
  <a href="https://zgojcic.github.io/" target="_blank">Zan&nbsp;Gojcic</a> &emsp; 
  <a href="https://orlitany.github.io/" target="_blank">Or&nbsp;Litany</a> &emsp; 
  <a href="https://www.cs.utoronto.ca/~fidler/" target="_blank">Sanja&nbsp;Fidler</a> &emsp; 
  <a href="https://karstenkreis.github.io/" target="_blank">Karsten&nbsp;Kreis</a>
  <br> <br>
  <a href="https://arxiv.org/abs/2210.06978" target="_blank">Paper</a> &emsp;
  <a href="https://nv-tlabs.github.io/LION" target="_blank">Project&nbsp;Page</a> 
</div>

<p align="center">
    <img width="750" alt="Animation" src="assets/animation.gif"/>
</p>

## Update
* When opening an issue, please add @ZENGXH so that I can reponse faster! 

## Install 
* Dependencies: 
    * CUDA 11.6 
    
* Setup the environment 
    Install from conda file  
    ``` 
        conda env create --name lion_env --file=env.yaml 
        conda activate lion_env 

        # Install some other packages 
        pip install git+https://github.com/openai/CLIP.git 

        # build some packages first (optional)
        python build_pkg.py
    ```
    Tested with conda version 22.9.0

* Using Docker
    * build the docker with `bash ./docker/build_docker.sh`
    * launch the docker with `bash ./docker/run.sh`


## Demo
run `python demo.py`, will load the released text2shape model on hugging face and generate a chair point cloud. (Note: the checkpoint is not released yet, the files loaded in the `demo.py` file is not available at this point)

## Released checkpoint and samples 
* will be release soon
* put the downloaded file under `./lion_ckpt/`

## Training 

### data 
* ShapeNet can be downloaded [here](https://github.com/stevenygd/PointFlow#dataset). 
* Put the downloaded data as `./data/ShapeNetCore.v2.PC15k` *or* edit the `pointflow` entry in `./datasets/data_path.py` for the ShapeNet dataset path. 

### train VAE 
* run `bash ./script/train_vae.sh $NGPU` (the released checkpoint is trained with `NGPU=4` on A100) 
* if want to use comet to log the experiment, add `.comet_api` file under the current folder, write the api key as `{"api_key": "${COMET_API_KEY}"}` in the `.comet_api` file

### train diffusion prior 
* require the vae checkpoint
* run `bash ./script/train_prior.sh $NGPU` (the released checkpoint is trained with `NGPU=8` with 2 node on V100)

### evaluate a trained prior 
* download the test data from [here](https://drive.google.com/file/d/1uEp0o6UpRqfYwvRXQGZ5ZgT1IYBQvUSV/view?usp=share_link), unzip and put it as `./datasets/test_data/`
* download the released checkpoint from above
```
checkpoint="./lion_ckpt/unconditional/airplane/checkpoints/model.pt" 
bash ./script/eval.sh $checkpoint  # will take 1-2 hour 
```

## Evaluate the samples with the 1-NNA metrics 
* download the test data from [here](https://drive.google.com/file/d/1uEp0o6UpRqfYwvRXQGZ5ZgT1IYBQvUSV/view?usp=share_link), unzip and put it as `./datasets/test_data/`
* run `python ./script/compute_score.py`

## Citation
```
@inproceedings{zeng2022lion,
    title={LION: Latent Point Diffusion Models for 3D Shape Generation},
        author={Xiaohui Zeng and Arash Vahdat and Francis Williams and Zan Gojcic and Or Litany and Sanja Fidler and Karsten Kreis},
        booktitle={Advances in Neural Information Processing Systems (NeurIPS)},
        year={2022}
}
```
