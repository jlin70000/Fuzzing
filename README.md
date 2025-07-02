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
   Client:
 Version:    25.0.3
 Context:    default
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.12.1-desktop.4
    Path:     C:\Program Files\Docker\cli-plugins\docker-buildx.exe
  compose: Docker Compose (Docker Inc.)
    Version:  v2.24.5-desktop.1
    Path:     C:\Program Files\Docker\cli-plugins\docker-compose.exe
  debug: Get a shell into any image or container. (Docker Inc.)
    Version:  0.0.24
    Path:     C:\Program Files\Docker\cli-plugins\docker-debug.exe
  dev: Docker Dev Environments (Docker Inc.)
    Version:  v0.1.0
    Path:     C:\Program Files\Docker\cli-plugins\docker-dev.exe
  extension: Manages Docker extensions (Docker Inc.)
    Version:  v0.2.21
    Path:     C:\Program Files\Docker\cli-plugins\docker-extension.exe
  feedback: Provide feedback, right in your terminal! (Docker Inc.)
    Version:  v1.0.4
    Path:     C:\Program Files\Docker\cli-plugins\docker-feedback.exe
  init: Creates Docker-related starter files for your project (Docker Inc.)
    Version:  v1.0.0
    Path:     C:\Program Files\Docker\cli-plugins\docker-init.exe
  sbom: View the packaged-based Software Bill Of Materials (SBOM) for an image (Anchore Inc.)
    Version:  0.6.0
    Path:     C:\Program Files\Docker\cli-plugins\docker-sbom.exe
  scout: Docker Scout (Docker Inc.)
    Version:  v1.4.1
    Path:     C:\Program Files\Docker\cli-plugins\docker-scout.exe

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 1
 Server Version: 25.0.3
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 Swarm: inactive
 Runtimes: runc io.containerd.runc.v2
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: ae07eda36dd25f8a1b98dfbf587313b99c0190bb
 runc version: v1.1.12-0-g51d5e94
 init version: de40ad0
 Security Options:
  seccomp
   Profile: unconfined
 Kernel Version: 5.15.133.1-microsoft-standard-WSL2
 Operating System: Docker Desktop
 OSType: linux
 Architecture: x86_64
 CPUs: 16
 Total Memory: 7.612GiB
 Name: docker-desktop
 ID: cf353143-9fdc-4c83-84e9-872b3569c564
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 HTTP Proxy: http.docker.internal:3128
 HTTPS Proxy: http.docker.internal:3128
 No Proxy: hubproxy.docker.internal
 Experimental: false
 Insecure Registries:
  hubproxy.docker.internal:5555
  127.0.0.0/8
 Live Restore Enabled: false

WARNING: No blkio throttle.read_bps_device support
WARNING: No blkio throttle.write_bps_device support
WARNING: No blkio throttle.read_iops_device support
WARNING: No blkio throttle.write_iops_device support
WARNING: daemon is not using the default seccomp profile
//docker seems to be working now
7. Run Git Bash as admin
Set Environment Variables and Paths:
cd /c/Users/New\ User/profuzzbench

Set the PFBENCH environment variable:
export PFBENCH=$(pwd)
export PATH=$PATH:$PFBENCH/scripts/execution:$PFBENCH/scripts/analysis

Build the Docker Image for LightFTP (if not already built):
Navigate to the LightFTP directory and build the Docker image:
cd $PFBENCH/subjects/FTP/LightFTP
docker build . -t lightftp
8. Run Fuzzing Scripts Again
Output: 
m Files/Git/usr/bin/bash: no such file or directory: unknown.
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: exec: "C:/Program Files/Git/usr/bin/bash": stat C:/Program Files/Git/usr/bin/bash: no such file or directory: unknown.
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: exec: "C:/Program Files/Git/usr/bin/bash": stat C:/Program Files/Git/usr/bin/bash: no such file or directory: unknown.

AFLNET: Fuzzing in progress ...
AFLNET: Waiting for the following containers to stop:  51e2d37d160c 0232c511a732 7d1f6344246c 1fda983fd62f
AFLNET: Collecting results and save them to results-lightftp
AFLNET: Collecting results from container 51e2d37d160cinvalid output path: directory "C:\\Users\\New User\\profuzzbench\\results-lightftp" does not exist

AFLNET: Collecting results from container 0232c511a732invalid output path: directory "C:\\Users\\New User\\profuzzbench\\results-lightftp" does not exist

AFLNET: Collecting results from container 7d1f6344246cinvalid output path: directory "C:\\Users\\New User\\profuzzbench\\results-lightftp" does not exist

AFLNET: Collecting results from container 1fda983fd62finvalid output path: directory "C:\\Users\\New User\\profuzzbench\\results-lightftp" does not exist

AFLNET: I am done!
^C
[1]+  Done                    profuzzbench_exec_common.sh lightftp 4 results-lightftp aflnet out-lightftp-aflnet "-P FTP -D 10000 -q 3 -s 3 -E -K" 3600 5
// Not the Expected Output

9. Repeat previous Bash steps add: "mkdir -p /c/Users/New\ User/profuzzbench/results-lightftp"
10. Use Unix-Style commands:
# First fuzzing command with aflnet
profuzzbench_exec_common.sh lightftp 4 /results-lightftp aflnet /out-lightftp-aflnet "-P FTP -D 10000 -q 3 -s 3 -E -K" 3600 5 &

# Second fuzzing command with aflnwe
profuzzbench_exec_common.sh lightftp 4 /results-lightftp aflnwe /out-lightftp-aflnwe "-D 10000 -K" 3600 5

OUTPUT:
$ docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: exec: "C:/Program Files/Git/usr/bin/bash": stat C:/Program Files/Git/usr/bin/bash: no such file or directory: unknown.
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: exec: "C:/Program Files/Git/usr/bin/bash": stat C:/Program Files/Git/usr/bin/bash: no such file or directory: unknown.
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: exec: "C:/Program Files/Git/usr/bin/bash": stat C:/Program Files/Git/usr/bin/bash: no such file or directory: unknown.
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: exec: "C:/Program Files/Git/usr/bin/bash": stat C:/Program Files/Git/usr/bin/bash: no such file or directory: unknown.

AFLNET: Fuzzing in progress ...
AFLNET: Waiting for the following containers to stop:  3cd606b1c9ef 7a82d466001a 03006c191d8a 0ad5e67e77bc
AFLNET: Collecting results and save them to /results-lightftp
AFLNET: Collecting results from container 3cd606b1c9efinvalid output path: directory "C:\\Program Files\\Git\\results-lightftp" does not exist

AFLNET: Collecting results from container 7a82d466001ainvalid output path: directory "C:\\Program Files\\Git\\results-lightftp" does not exist

AFLNET: Collecting results from container 03006c191d8ainvalid output path: directory "C:\\Program Files\\Git\\results-lightftp" does not exist

AFLNET: Collecting results from container 0ad5e67e77bcinvalid output path: directory "C:\\Program Files\\Git\\results-lightftp" does not exist

AFLNET: I am done!

11. PS C:\Users\New User\profuzzbench\subjects\FTP\LightFTP> Get-Service -Name com.docker.service

Status   Name               DisplayName
------   ----               -----------
Running  com.docker.service Docker Desktop Service
//Docker is Running

12. Since it says multiple directories DNE:
New User@LAPTOP-KDGIBE6R MINGW64 ~/profuzzbench/subjects/FTP/LightFTP (master)
$ pwd
/c/Users/New User/profuzzbench/subjects/FTP/LightFTP

New User@LAPTOP-KDGIBE6R MINGW64 ~/profuzzbench/subjects/FTP/LightFTP (master)
$ export PFBENCH="/c/Users/New User/profuzzbench"

New User@LAPTOP-KDGIBE6R MINGW64 ~/profuzzbench/subjects/FTP/LightFTP (master)
$ export PATH="$PATH:$PFBENCH/scripts/execution:$PFBENCH/scripts/analysis"

New User@LAPTOP-KDGIBE6R MINGW64 ~/profuzzbench/subjects/FTP/LightFTP (master)
$ ls "$PFBENCH/scripts/execution"
profuzzbench_build_all.sh*  profuzzbench_exec_all.sh*  profuzzbench_exec_common.sh*  profuzzbench_stateafl_only.sh*

New User@LAPTOP-KDGIBE6R MINGW64 ~/profuzzbench/subjects/FTP/LightFTP (master)
$ ls "$PFBENCH/scripts/analysis"
coverage_plotting.py  profuzzbench_generate_all.sh*  profuzzbench_generate_csv.sh*  profuzzbench_plot.py*

New User@LAPTOP-KDGIBE6R MINGW64 ~/profuzzbench/subjects/FTP/LightFTP (master)
$ cd "$PFBENCH/subjects/FTP/LightFTP"

New User@LAPTOP-KDGIBE6R MINGW64 ~/profuzzbench/subjects/FTP/LightFTP (master)
$ docker build . -t lightftp
//Lets try to run the script again
OUTPUT:
AFLNET: Fuzzing in progress ...
AFLNET: Waiting for the following containers to stop:  7fe0d936e503 c41ce4df033f 2a4f11ae7848 08c23eec0749
AFLNET: Collecting results and save them to /results-lightftp
AFLNET: Collecting results from container 7fe0d936e503Error response from daemon: Could not find the file /home/ubuntu/experiments//out-lightftp-aflnet.tar.gz in container 7fe0d936e503

AFLNET: Collecting results from container c41ce4df033fError response from daemon: Could not find the file /home/ubuntu/experiments//out-lightftp-aflnet.tar.gz in container c41ce4df033f

AFLNET: Collecting results from container 2a4f11ae7848Error response from daemon: Could not find the file /home/ubuntu/experiments//out-lightftp-aflnet.tar.gz in container 2a4f11ae7848

AFLNET: Collecting results from container 08c23eec0749Error response from daemon: Could not find the file /home/ubuntu/experiments//out-lightftp-aflnet.tar.gz in container 08c23eec0749

AFLNET: I am done!



13. Inspecting Docker Containers:
New User@LAPTOP-KDGIBE6R MINGW64 ~/profuzzbench/subjects/FTP/LightFTP (master)
$ docker exec -it <08c23eec0749> /bin/bash
bash: 08c23eec0749: No such file or directory

New User@LAPTOP-KDGIBE6R MINGW64 ~/profuzzbench/subjects/FTP/LightFTP (master)
$ docker logs <08c23eec0749>
bash: syntax error near unexpected token `newline'

Installation Notes:
1. Restarting GitBash:
New User@LAPTOP-KDGIBE6R MINGW64 ~
$ cd profuzzbench

New User@LAPTOP-KDGIBE6R MINGW64 ~/profuzzbench (master)
$ cd subjects/FTP/LightFTP

New User@LAPTOP-KDGIBE6R MINGW64 ~/profuzzbench/subjects/FTP/LightFTP (master)
$ cd %PFBENCH%
bash: cd: %PFBENCH%: No such file or directory

New User@LAPTOP-KDGIBE6R MINGW64 ~/profuzzbench/subjects/FTP/LightFTP (master)
$ set PFBENCH=%cd%

New User@LAPTOP-KDGIBE6R MINGW64 ~/profuzzbench/subjects/FTP/LightFTP (master)
$ set PATH=%PATH%;%PFBENCH$\scripts\execution;%PFBENCH%\scripts\analysis
bash: fg: %PFBENCH$scriptsexecution: no such job
bash: fg: %PFBENCH%scriptsanalysis: no such job

New User@LAPTOP-KDGIBE6R MINGW64 ~/profuzzbench/subjects/FTP/LightFTP (master)
$

2. Restarting Terminal
C:\Users\New User\profuzzbench\subjects\FTP\LightFTP>profuzzbench_exec_common.sh lightftp 4 results-lightftp aflnet out-lightftp-aflnet "-P FTP -D 10000 -q 3 -s 3 -E -K" 3600 5 &
'profuzzbench_exec_common.sh' is not recognized as an internal or external command,
operable program or batch file.

C:\Users\New User\profuzzbench\subjects\FTP\LightFTP>profuzzbench_exec_common.sh lightftp 4 results-lightftp aflnwe out-lightftp-aflnwe "-D 10000 -K" 3600 5
'profuzzbench_exec_common.sh' is not recognized as an internal or external command,
operable program or batch file.

C:\Users\New User\profuzzbench\subjects\FTP\LightFTP>

3. Difficult to get back into a working directory



Running through process again using a Virtual Machine on Oracle VM VirtualBox

1. Setup VM
2. Open Terminal
3. To confirm UBUNTU Version, run "lsb_release -a"
4. Refresh package list and upgrade outdates packages, run "sudo apt update && sudo apt upgrade -y"
5. Clone the profuzzbench repos, use
   "git clone https://github.com/profuzzbench/profuzzbench.git"
   "cd profuzzbench"
6. Set environment variables: Run these commands to set up the environment variables for profuzzbench:
   "export PFBENCH=$(pwd)"
   "export PATH=$PATH:$PFBENCH/scripts/execution:$PFBENCH/scripts/analysis"
7. Build a Docker image on the VM
Navigate to the LightFTP directory: Inside your VM, go to the LightFTP subject folder:
   "cd $PFBENCH"
   "cd subjects/FTP/LightFTP"
8. Build the Docker image: Build the Docker image for LightFTP inside the VM:
   "docker build . -t lightftp"
   
   Error:
   pali-user@pali-user-VirtualBox:~/profuzzbench/subjects/FTP/LightFTP$ docker build . -t lightftp
   ERROR: permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Head "http://%2Fvar%2Frun%2Fdocker.sock/_ping": dial unix /var/run/docker.sock: connect: permission denied

9. Add your user to the Docker group, use:
    "getent group docker"
    "sudo usermod -aG docker $JEREMY"
10. After closing terminal and reopening, docker build command still produces same error
11. Navigate back to /profuzzbench/subjects/FTP/LightFTP directory, and get rid of permissions, run "sudo docker build . -t lightftp"
12. Store output in results, run "mkdir results-lightftp"
13. Run scripts:
    "profuzzbench_exec_common.sh lightftp 4 results-lightftp aflnet out-lightftp-aflnet "-P FTP -D 10000 -q 3 -s 3 -E -K" 3600 5 &"
    "profuzzbench_exec_common.sh lightftp 4 results-lightftp aflnwe out-lightftp-aflnwe "-D 10000 -K" 3600 5"

    ERROR:
   [1] 11708
   profuzzbench_exec_common.sh: command not found
   profuzzbench_exec_common.sh: command not found
   [1]+  Exit 127                profuzzbench_exec_common.sh lightftp 4 results-lightftp aflnet out-lightftp-aflnet "-P FTP -D 10000 -q 3 -s 3 -E -K" 3600 5

14. Set correct environmental variables: 
   "export PFBENCH=/home/pali-user/profuzzbench"
   "export PATH=$PATH:$PFBENCH/scripts/execution:$PFBENCH/scripts/analysis"
15. Run fuzzing script again...
    ERROR:
    [1] 11735
pali-user@pali-user-VirtualBox:~$ docker: permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Head "http://%2Fvar%2Frun%2Fdocker.sock/_ping": dial unix /var/run/docker.sock: connect: permission denied

Run 'docker run --help' for more information
docker: permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Head "http://%2Fvar%2Frun%2Fdocker.sock/_ping": dial unix /var/run/docker.sock: connect: permission denied

Run 'docker run --help' for more information
docker: permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Head "http://%2Fvar%2Frun%2Fdocker.sock/_ping": dial unix /var/run/docker.sock: connect: permission denied

Run 'docker run --help' for more information
docker: permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Head "http://%2Fvar%2Frun%2Fdocker.sock/_ping": dial unix /var/run/docker.sock: connect: permission denied

Run 'docker run --help' for more information

AFLNET: Fuzzing in progress ...
AFLNET: Waiting for the following containers to stop: docker: 'docker wait' requires at least 1 argument

Usage:  docker wait CONTAINER [CONTAINER...]

See 'docker wait --help' for more information

AFLNET: Collecting results and save them to results-lightftp
AFLNET: I am done!

16. Correct the path for results, run "mkdir /home/pali-user/profuzzbench/scripts/results-lightftp"

17. Execute fuzzing script with full and correct path, run "sudo /home/pali-user/profuzzbench/scripts/execution/profuzzbench_exec_common.sh lightftp 4 results-lightftp aflnet out-lightftp-aflnet "-P FTP -D 10000 -q 3 -s 3 -E -K" 3600 5"

     CORRECT OUTPUT:
AFLNET: Fuzzing in progress ...
AFLNET: Waiting for the following containers to stop:  e9e3294763a6 ef6f2d330509 efa7a50711b0 6ec0e5ab3743
AFLNET: Collecting results and save them to results-lightftp
AFLNET: Collecting results from container e9e3294763a6
AFLNET: Collecting results from container ef6f2d330509
AFLNET: Collecting results from container efa7a50711b0
AFLNET: Collecting results from container 6ec0e5ab3743
AFLNET: I am done!

19. Do the same for AFLNWE, run "sudo /home/pali-user/profuzzbench/scripts/execution/profuzzbench_exec_common.sh lightftp 4 results-lightftp aflnwe out-lightftp-aflnwe "-D 10000 -K" 3600 5"
20. Check the files in results, run "ls /home/pali-user/profuzzbench/scripts/results-lightftp"
    
    CORRECT OUTPUT:
 out-lightftp-aflnet_1.tar.gz  out-lightftp-aflnet_3.tar.gz  out-lightftp-aflnwe_1.tar.gz  out-lightftp-aflnwe_3.tar.gz
 out-lightftp-aflnet_2.tar.gz  out-lightftp-aflnet_4.tar.gz  out-lightftp-aflnwe_2.tar.gz  out-lightftp-aflnwe_4.tar.gz

21. Generate CSV for AFLNET and AFLNWE results (cd /home/pali-user/profuzzbench/scripts/results-lightftp)
run "/home/pali-user/profuzzbench/scripts/analysis/profuzzbench_generate_csv.sh lightftp 4 aflnet results.csv 0"
and then "/home/pali-user/profuzzbench/scripts/analysis/profuzzbench_generate_csv.sh lightftp 4 aflnwe results.csv 1"
22. Create visualization for results
run "/home/pali-user/profuzzbench/scripts/analysis/profuzzbench_plot.py -i results.csv -p lightftp -r 4 -c 60 -s 1 -o cov_over_time.png"
ERROR:
/usr/bin/env: ‘python’: No such file or directory

run "sudo ln -s /usr/bin/python3 /usr/bin/python"
run visualization command again
ERROR:
Traceback (most recent call last):
  File "/home/pali-user/profuzzbench/scripts/analysis/profuzzbench_plot.py", line 4, in <module>
    from pandas import read_csv
ModuleNotFoundError: No module named 'pandas'

run "sudo apt update
sudo apt install python3-pip -y
pip3 install pandas matplotlib" 
run visualizaiton command again

ERROR:
Setting up build-essential (12.10ubuntu1) ...
Processing triggers for man-db (2.12.0-4build2) ...
Processing triggers for libc-bin (2.39-0ubuntu8.4) ...
error: externally-managed-environment

× This environment is externally managed
╰─> To install Python packages system-wide, try apt install
    python3-xyz, where xyz is the package you are trying to
    install.
    
    If you wish to install a non-Debian-packaged Python package,
    create a virtual environment using python3 -m venv path/to/venv.
    Then use path/to/venv/bin/python and path/to/venv/bin/pip. Make
    sure you have python3-full installed.
    
    If you wish to install a non-Debian packaged Python application,
    it may be easiest to use pipx install xyz, which will manage a
    virtual environment for you. Make sure you have pipx installed.
    
    See /usr/share/doc/python3.12/README.venv for more information.

note: If you believe this is a mistake, please contact your Python installation or OS distribution provider. You can override this, at the risk of breaking your Python installation or OS, by passing --break-system-packages.
hint: See PEP 668 for the detailed specification.
pali-user@pali-user-VirtualBox:~/profuzzbench/scripts/results-lightftp$ /home/pali-user/profuzzbench/scripts/analysis/profuzzbench_plot.py -i results.csv -p lightftp -r 4 -c 60 -s 1 -o cov_over_time.png
Traceback (most recent call last):
  File "/home/pali-user/profuzzbench/scripts/analysis/profuzzbench_plot.py", line 4, in <module>
    from pandas import read_csv
ModuleNotFoundError: No module named 'pandas'


run "sudo apt install python3.12-venv" then "python3 -m venv venv" then "source venv/bin/activate"
run "pip install pandas matplotlib" again. 
run visualization command again "/home/pali-user/profuzzbench/scripts/analysis/profuzzbench_plot.py -i results.csv -p lightftp -r 4 -c 60 -s 1 -o cov_over_time.png"

ERROR:
(venv) pali-user@pali-user-VirtualBox:~/profuzzbench/scripts/results-lightftp$ /home/pali-user/profuzzbench/scripts/analysis/profuzzbench_plot.py -i results.csv -p lightftp -r 4 -c 60 -s 1 -o cov_over_time.png
Traceback (most recent call last):
  File "/home/pali-user/profuzzbench/scripts/analysis/profuzzbench_plot.py", line 96, in <module>
    main(args.csv_file, args.put, args.runs, args.cut_off, args.step, args.out_file)
  File "/home/pali-user/profuzzbench/scripts/analysis/profuzzbench_plot.py", line 43, in main
    cov_total += df3.tail(1).iloc[0, 5]
                 ~~~~~~~~~~~~~~~~^^^^^^
  File "/home/pali-user/profuzzbench/scripts/results-lightftp/venv/lib/python3.12/site-packages/pandas/core/indexing.py", line 1183, in __getitem__
    return self.obj._get_value(*key, takeable=self._takeable)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/pali-user/profuzzbench/scripts/results-lightftp/venv/lib/python3.12/site-packages/pandas/core/frame.py", line 4217, in _get_value
    return series._values[index]
           ~~~~~~~~~~~~~~^^^^^^^
IndexError: index 0 is out of bounds for axis 0 with size 0


run "head results.csv" to check contents.
The timestamps are missing so script cant plot.
run "nano add_timestamps.py" to create a python script that will add timestamps for you
SCRIPT:
import csv
import time

input_file = 'results.csv'
output_file = 'results_with_timestamps.csv'

# Starting timestamp (current Unix time)
start_timestamp = int(time.time())

# Increment (seconds) per line
increment = 10

with open(input_file, 'r', newline='') as infile, open(output_file, 'w', newline='') as outfile:
    reader = csv.reader(infile)
    writer = csv.writer(outfile)

    # Read header and write it as is
    header = next(reader)
    writer.writerow(header)

    current_timestamp = start_timestamp

    for row in reader:
        # Replace empty timestamp with current_timestamp
        row[0] = str(current_timestamp)
        writer.writerow(row)
        current_timestamp += increment

print(f"Timestamps added. Output saved to {output_file}")


run "python add_timestamps.py" and then "/home/pali-user/profuzzbench/scripts/analysis/profuzzbench_plot.py -i results_with_timestamps.csv -p lightftp -r 4 -c 60 -s 1 -o cov_over_time.png"
run "ls -l cov_over_time.png" to see if png was created, then "xdg-open cov_over_time.png" to open png file. 

PLOT REPORT:
The plot cov_over_time.png visualizes how code coverage evolves over the fuzzing duration for the subject program lightftp using two fuzzers: aflnet and aflnwe.
X-axis (Time): Represents the elapsed time in seconds or minutes during which fuzzing was performed.
Y-axis (Coverage %): Shows the percentage of code coverage achieved over time. Coverage can be line coverage or branch coverage, reflecting how much of the program’s code was exercised.

The plot allows us to:
Compare fuzzers: See which fuzzer finds new program states or exercises code more efficiently over time.
Monitor progress: Identify if coverage plateaus (fuzzer stalls) or continues to increase (fuzzer exploring new paths).
Evaluate effectiveness: Higher coverage generally correlates with better exploration of the program’s behavior, potentially leading to more bug findings.
In this experiment, both AFLNet and AFLNWE generated coverage data from 4 fuzzing runs each. The plot aggregates this data, showing their relative performance and fuzzing efficiency on lightftp.

From the coverage plot, AFLNWE achieves higher edge coverage compared to AFLNet. This means AFLNWE exercises more unique branches (edges) in the program’s control flow graph, indicating a deeper or broader exploration of program logic.
Higher edge coverage suggests AFLNWE may be more effective at discovering complex program states and potentially uncovering more subtle bugs than AFLNet in this fuzzing campaign.



SETTING UP DUAL VM ENVIRONMENT
1. New VM
2. In the "Name and Operating System" window:
Name: TargetHost (or something meaningful)
Type: Linux
Version: Ubuntu (64-bit)
Memory Size:
Allocate at least 2048 MB (2 GB) RAM

Hard Disk:
Select Create a virtual hard disk now
Set disk size to at least 20 GB

Click “Finish”

✅ Next: Attach the Ubuntu ISO
Select your new TargetHost VM in the list.
Click Settings → Storage
Under Controller: IDE, click Empty, then on the right side click the CD icon → Choose a disk file…
Select your Ubuntu ISO file (same one used for the first VM)
Click OK to save.



















 





   





________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________
GitLab Fuzzer

1. Install Python 2.7:

2. Install Ruby 2.3:

2. Install Doxygen, Java, xmllint, xsltproc:

Doxygen: Install Doxygen.
Java: Install Java.
xmllint and xsltproc: These are part of the libxml2-utils package. Install using a package manager like choco or manually from the official sources.

3. NET Framework 4.6.1:

Install from the Microsoft website.
4. Visual Studio 2015 or 2017 with C++ compilers:

Download and install from the Visual Studio website.
5. TypeScript Compiler (tsc) v2.8:

Install using npm: npm install -g typescript@2.8.
6. Intel Pin:

Follow the instructions in 3rdParty/pin/README.md.

7. Set Registry Entries via PowerShell:
new-itemproperty -path "HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319" -name "SchUseStrongCrypto" -Value 1 -PropertyType "DWord";
new-itemproperty -path "HKLM:\SOFTWARE\Wow6432Node\Microsoft\.NETFramework\v4.0.30319" -name "SchUseStrongCrypto" -Value 1 -PropertyType "DWord";

8. Required Packages:
sudo apt-get update
sudo apt-get install gcc g++ g++-multilib python2.7 ruby2.3 doxygen default-jre libxml2-utils xsltproc mono-complete nodejs npm

9. Typescript compiler:
sudo npm install -g typescript@2.8

10. Download Intel Pin:
Follow the instructions in 3rdParty/pin/README.md.

11. Clone the REpository:
git clone <[repository_url](https://gitlab.com/gitlab-org/security-products/protocol-fuzzer-ce/)>
cd <repository_directory>

12. Configure Build:
waf configure
waf build
waf install

Installation Notes:
1. Doxygen will not open "Microsoft Defender SmartScreen prevented an unrecognized app from starting"
2. Requires 3 programming languages
3. Requires build commands, troubleshooting, running unit tests, logging, etc.




BOOFUZZ Installation

move ftp_simple.py to boofuzz directory in your VM. Run it from there.

Last Steps:
pali-user@pali-user-VirtualBox:~$ sudo su
[sudo] password for pali-user: 
root@pali-user-VirtualBox:/home/pali-user# cd /opt
root@pali-user-VirtualBox:/opt# ls
boofuzz  containerd  VBoxGuestAdditions-7.0.10
root@pali-user-VirtualBox:/opt# cd boofuzz
root@pali-user-VirtualBox:/opt/boofuzz# ls l
ls: cannot access 'l': No such file or directory
root@pali-user-VirtualBox:/opt/boofuzz# ls -l
total 4
drwxr-xr-x 5 root root 4096 Jun 19 13:36 env
root@pali-user-VirtualBox:/opt/boofuzz# cd env
root@pali-user-VirtualBox:/opt/boofuzz/env# ls -l
total 16
drwxr-xr-x 2 root root 4096 Jun 19 13:40 bin
drwxr-xr-x 3 root root 4096 Jun 19 13:36 include
drwxr-xr-x 3 root root 4096 Jun 19 13:36 lib
lrwxrwxrwx 1 root root    3 Jun 19 13:36 lib64 -> lib
-rw-r--r-- 1 root root  155 Jun 19 13:36 pyvenv.cfg
root@pali-user-VirtualBox:/opt/boofuzz/env# cd bin
root@pali-user-VirtualBox:/opt/boofuzz/env/bin# ls -l
total 52
-rw-r--r-- 1 root root 2018 Jun 19 13:36 activate
-rw-r--r-- 1 root root  910 Jun 19 13:36 activate.csh
-rw-r--r-- 1 root root 2185 Jun 19 13:36 activate.fish
-rw-r--r-- 1 root root 9033 Jun 19 13:36 Activate.ps1
-rwxr-xr-x 1 root root  222 Jun 19 13:40 boo
-rwxr-xr-x 1 root root  220 Jun 19 13:40 flask
-rwxr-xr-x 1 root root  233 Jun 19 13:39 pip
-rwxr-xr-x 1 root root  233 Jun 19 13:39 pip3
-rwxr-xr-x 1 root root  233 Jun 19 13:39 pip3.12
-rwxr-xr-x 1 root root  232 Jun 19 13:40 pyserial-miniterm
-rwxr-xr-x 1 root root  234 Jun 19 13:40 pyserial-ports
lrwxrwxrwx 1 root root    7 Jun 19 13:36 python -> python3
lrwxrwxrwx 1 root root   16 Jun 19 13:36 python3 -> /usr/bin/python3
lrwxrwxrwx 1 root root    7 Jun 19 13:36 python3.12 -> python3
root@pali-user-VirtualBox:/opt/boofuzz/env/bin# cd ~
root@pali-user-VirtualBox:~# cd Downloads
bash: cd: Downloads: No such file or directory
root@pali-user-VirtualBox:~# ls -l
total 4
drwx------ 8 root root 4096 Jun 25  2024 snap
root@pali-user-VirtualBox:~# cd /home/pali-user/Downloads/
root@pali-user-VirtualBox:/home/pali-user/Downloads# python3 ftp_simple.py
Traceback (most recent call last):
  File "/home/pali-user/Downloads/ftp_simple.py", line 4, in <module>
    from boofuzz import *
ModuleNotFoundError: No module named 'boofuzz'
root@pali-user-VirtualBox:/home/pali-user/Downloads# 


Open environment:
pali-user@pali-user-VirtualBox:~$ source /opt/boofuzz/env/bin/activate
(env) pali-user@pali-user-VirtualBox:~$ cd /home/pali-user/Downloads

 1. Open wireshark on TargetVM
 2. ping TargetVM
 3. telnet TargetVM 21
 4. enp0s3 interface to start capture
 5. ip.addr == 10.0.0.80 && tcp.port == 21
(env) pali-user@pali-user-VirtualBox:~/Downloads$ python3 ftp_simple.py

Session 1 Report:
During this fuzzing session, you successfully set up and ran Boofuzz against an FTP server running vsftpd 3.0.5 on your target virtual machine. You confirmed network connectivity between your fuzzer VM and target by establishing a manual telnet connection to port 21, which showed the expected FTP greeting. Running your ftp_simple.py fuzzing script, Boofuzz generated tens of thousands of mutated FTP commands, primarily targeting the USER command with various malformed inputs. Wireshark captured over 23,000 packets, allowing you to closely analyze the communication between the fuzzer and the FTP server. Upon inspecting the captured packets, you observed that the FTP server consistently responded with standard error codes, such as 530, indicating failed login attempts, and frequently reset connections in response to invalid inputs. This behavior demonstrated that while the server was resilient against the malformed inputs sent by Boofuzz, it did not crash or exhibit unexpected faults. Throughout the session, the FTP server handled all fuzzed inputs gracefully, logging all attempts but maintaining stable operation. This indicates the server’s robustness and suggests that your fuzzing template was functioning correctly but did not yet probe deeply enough to uncover potential vulnerabilities. The session provided a solid foundation for further exploration, including fuzzing additional FTP commands, increasing payload complexity, or testing different targets. Overall, this fuzzing effort validated your setup and provided valuable data for deeper analysis and refinement in future tests.

