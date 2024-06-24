# Fuzzing


Profuzzbench Installation

Microsoft Windows [Version 10.0.22631.3155]
(c) Microsoft Corporation. All rights reserved.

C:\Users\New User>cd subjects/FTP/LightFTP
The system cannot find the path specified.

C:\Users\New User>cd profuzzbench

C:\Users\New User\profuzzbench>cd %PFBENCH%
The system cannot find the path specified.

C:\Users\New User\profuzzbench>set PFBENCH=%cd%

C:\Users\New User\profuzzbench>set PATH=%PATH%;%PFBENCH%\scripts\execution;%PFBENCH%\scripts\analysis

C:\Users\New User\profuzzbench>cd %PFBENCH%

C:\Users\New User\profuzzbench>cd subjects/FTP/LightFTP

C:\Users\New User\profuzzbench\subjects\FTP\LightFTP>docker build . -t lightftp
[+] Building 158.0s (26/26) FINISHED                                                                     docker:default
 => [internal] load build definition from Dockerfile                                                               0.0s
 => => transferring dockerfile: 3.57kB                                                                             0.0s
 => [internal] load metadata for docker.io/library/ubuntu:20.04                                                    1.0s
 => [internal] load .dockerignore                                                                                  0.0s
 => => transferring context: 2B                                                                                    0.0s
 => [ 1/21] FROM docker.io/library/ubuntu:20.04@sha256:bb1c41682308d7040f74d103022816d41c50d7b0c89e9d706a74b4e548  4.4s
 => => resolve docker.io/library/ubuntu:20.04@sha256:bb1c41682308d7040f74d103022816d41c50d7b0c89e9d706a74b4e54863  0.0s
 => => sha256:bb1c41682308d7040f74d103022816d41c50d7b0c89e9d706a74b4e548636e54 1.13kB / 1.13kB                     0.0s
 => => sha256:a4fab1802f08df089c4b2e0a1c8f1a06f573bd1775687d07fef4076d3a2e4900 424B / 424B                         0.0s
 => => sha256:18ca3f4297e795532c0d053ba443d392d5d316ee83ddee0de27f1e742a7db273 2.30kB / 2.30kB                     0.0s
 => => sha256:8ee0874247356ecb5ea92128219660506b139dcb6cc45dcab84d98b3c6485061 27.51MB / 27.51MB                   3.5s
 => => extracting sha256:8ee0874247356ecb5ea92128219660506b139dcb6cc45dcab84d98b3c6485061                          0.8s
 => [internal] load build context                                                                                  0.1s
 => => transferring context: 12.30kB                                                                               0.0s
 => [ 2/21] RUN apt-get -y update &&     apt-get -y install sudo     apt-utils     build-essential     openssl   109.3s
 => [ 3/21] RUN groupadd ubuntu &&     useradd -rm -d /home/ubuntu -s /bin/bash -g ubuntu -G sudo -u 1000 ubuntu   0.3s
 => [ 4/21] RUN chmod 777 /tmp                                                                                     0.4s
 => [ 5/21] RUN pip3 install gcovr==4.2                                                                            3.3s
 => [ 6/21] WORKDIR /home/ubuntu                                                                                   0.0s
 => [ 7/21] RUN git clone https://github.com/profuzzbench/aflnet.git &&     cd aflnet &&     make clean all $MAK  18.3s
 => [ 8/21] RUN git clone https://github.com/profuzzbench/aflnwe.git &&     cd aflnwe &&     make clean all $MAK  10.3s
 => [ 9/21] RUN mkdir /home/ubuntu/experiments                                                                     0.4s
 => [10/21] COPY --chown=ubuntu:ubuntu fuzzing.patch /home/ubuntu/experiments/fuzzing.patch                        0.0s
 => [11/21] COPY --chown=ubuntu:ubuntu gcov.patch /home/ubuntu/experiments/gcov.patch                              0.0s
 => [12/21] RUN cd /home/ubuntu/experiments &&     git clone https://github.com/hfiref0x/LightFTP.git &&     cd L  2.0s
 => [13/21] RUN cd /home/ubuntu/experiments/LightFTP/Source/Release &&     cp /home/ubuntu/aflnet/tutorials/light  0.4s
 => [14/21] RUN cd /home/ubuntu/experiments &&     git clone https://github.com/hfiref0x/LightFTP.git LightFTP-gc  1.5s
 => [15/21] RUN cd /home/ubuntu/experiments/LightFTP-gcov/Source/Release &&     cp /home/ubuntu/aflnet/tutorials/  0.4s
 => [16/21] COPY --chown=ubuntu:ubuntu in-ftp /home/ubuntu/experiments/in-ftp                                      0.0s
 => [17/21] COPY --chown=ubuntu:ubuntu ftp.dict /home/ubuntu/experiments/ftp.dict                                  0.0s
 => [18/21] COPY --chown=ubuntu:ubuntu cov_script.sh /home/ubuntu/experiments/cov_script                           0.0s
 => [19/21] COPY --chown=ubuntu:ubuntu run.sh /home/ubuntu/experiments/run                                         0.0s
 => [20/21] COPY --chown=ubuntu:ubuntu clean.sh /home/ubuntu/experiments/ftpclean                                  0.0s
 => [21/21] RUN apt-get -y install ftp                                                                             2.3s
 => exporting to image                                                                                             3.4s
 => => exporting layers                                                                                            3.4s
 => => writing image sha256:4b59fa9659a8859e10e1e43a57ac274270112de6cb4c293ee62d07f435f8d569                       0.0s
 => => naming to docker.io/library/lightftp                                                                        0.0s

What's Next?
  1. Sign in to your Docker account → docker login
  2. View a summary of image vulnerabilities and recommendations → docker scout quickview

C:\Users\New User\profuzzbench\subjects\FTP\LightFTP>



Testing Profuzzbench

cd $PFBENCH //Current directory to variable 
mkdir results-lightftp //Creates new directory in current working directory to store results

profuzzbench_exec_common.sh lightftp 4 results-lightftp aflnet out-lightftp-aflnet "-P FTP -D 10000 -q 3 -s 3 -E -K" 3600 5 &
profuzzbench_exec_common.sh lightftp 4 results-lightftp aflnwe out-lightftp-aflnwe "-D 10000 -K" 3600 5

First Command (aflnet fuzzing):

"profuzzbench_exec_common.sh lightftp 4 results-lightftp aflnet out-lightftp-aflnet "-P FTP -D 10000 -q 3 -s 3 -E -K" 3600 5 &"
Arguments breakdown:
lightftp: Docker image name.
4: Number of runs (isolated Docker containers).
results-lightftp: Path to the folder where results will be saved.
aflnet: Fuzzer name.
out-lightftp-aflnet: Name of the output folder created inside the Docker container.
"-P FTP -D 10000 -q 3 -s 3 -E -K": Options needed for fuzzing AFLNet, specifying parameters like protocol (FTP), dictionary size (10000), and other specific options (-q 3 -s 3 -E -K).
3600: Timeout for fuzzing in seconds (1 hour).
5: Skip count for coverage calculation.
The & at the end runs this command in the background, allowing you to continue using the terminal while the fuzzing runs.

Second Command (aflnwe fuzzing):

"profuzzbench_exec_common.sh lightftp 4 results-lightftp aflnwe out-lightftp-aflnwe "-D 10000 -K" 3600 5"
Arguments breakdown:
lightftp: Docker image name (same as above).
4: Number of runs.
results-lightftp: Results directory.
aflnwe: Fuzzer name.
out-lightftp-aflnwe: Output folder name inside Docker.
"-D 10000 -K": Options for AFLnwe, specifying dictionary size (10000) and other options (-K).
3600: Timeout for fuzzing (1 hour).
5: Skip count for coverage calculation.

Immediate Results/Actions:
1. Git Bash opens black
2. New command opens in terminal
3. "jobs" to see if fuzzing is running in the background - no result
4. ps aux, docker ps, sudo systemct1 status docker, all do not yield results
5. docker --version in terminal to check if installed
6. docker info
7. 
