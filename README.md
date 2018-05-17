# Simpsons TV script generation

This repository shows an example implementation  of Udacity's Deep Learning Nanodegree project on RNNs. The idea here is that the RNN learns text from the Simpson show at 'Moe's Tavern' and from this new text can be generated. As the dataset is small, the text will be non-sensical, but the words as such make sense.

## Getting Started

The below instructions will show how I setup the environment to run this project on an Amazon GPU. It is assumed that you already have an AWS Account.

### AWS Environment Setup
1. Sign in to Console. Then go to _Services_ > _EC2_.
2. Click on _Launch Instance_ button.
3. Choose Amazon Machine Image (AMI) by selecting _AWS Marketplace_:
Search for _Deep Learning AMI with Source Code (CUDA 8, Ubuntu)_ and select it.
4. Choose an Instance Type (filter by _GPU compute_): Select p2.xlarge.
5. Click on _Review and Launch_.
6. Configure Security Groups.
7. Generate <your key name>.pem authentication key pair, if needed.
8. Click _Launch Instances_ and then _View Instances_ to come to the EC2 Dashboard.
9. When _View Instances_ displays _2/2 checks passed_ under _Status Checks_, proceed to next step.

### EC2 Instance Setup
1. Login to EC2 Instance:
```
ssh -i  <your key name>.pem ubuntu@<IPv4 public IP address displayed on EC2 Dashboard>
```
2. Clone the repository and navigate to the project folder:
```	
git clone https://github.com/gro1m/simpsons_tv_script.git
cd simpsons_tv_script
```
3. Install requirements from requirements.txt:
```
sudo python3 -m pip install -r requirements.txt
```
4. Install cudnn:
   1. Download and untar cudnn version 6 for CUDA 8 (Copied from [RISHAV KUMAR](https://stackoverflow.com/questions/31279494/how-to-install-cudnn-from-command-line):
   ```
   CUDNN_TAR_FILE="cudnn-8.0-linux-x64-v6.0.tgz"
   wget http://developer.download.nvidia.com/compute/redist/cudnn/v6.0/${CUDNN_TAR_FILE}
   tar -xzvf ${CUDNN_TAR_FILE}
   ```
   2. Copy the files to appropriate locations and set the correct rights - see also: [NVIDIA](https://docs.nvidia.com/deeplearning/sdk/cudnn-install/#installcuda):
   ```
   sudo cp -P cuda/include/cudnn.h /usr/local/cuda/include
   sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64
   sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
   ```
   3. Adjust cudnn library softlinks:
   ```
   cd /usr/local/cuda/lib64
   sudo ln -sf libcudnn.so.6 libcudnn.so
   sudo ln -sf libcudnn.so.6.0.21 libcudnn.so.6
   sudo ldconfig
   ```

__NOTE:__ I tried to use [cudnn6.5(v2)](http://developer.download.nvidia.com/compute/redist/cudnn/v2/cudnn-6.5-linux-x64-v2.tgz), but this led to an import error.

### Setup jupyter notebook
1. `cd /home/ubuntu`
2. `jupyter notebook --ip=0.0.0.0 --no-browser`
3. Need token by jupyter notebook token, so copy everything starting with `:8888/?token`
4. Access jupyter notebook index from your web-browser by visiting:
`<IPv4 public IP address displayed on EC2 Dashboard> :8888/?token=...`

## License

This project is licensed under the MIT License - see the [LICENSE.txt](LICENSE.txt) file for details.

## Acknowledgments

I acknowledge that this project is heavily based on the framework provided by Udacity on [tv_script_generation](https://github.com/udacity/deep-learning/tree/master/tv-script-generation).