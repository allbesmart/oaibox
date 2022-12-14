<table style="border-collapse: collapse; border: none;">
  <tr style="border-collapse: collapse; border: none;">
    <td style="border-collapse: collapse; border: none;">
      <a href="http://www.openairinterface.org/">
         <img src="../../doc/images/oai_final_logo.png" alt="" border=3 height=50 width=150>
         </img>
      </a>
    </td>
    <td style="border-collapse: collapse; border: none; vertical-align: center;">
      <b><font size = "5">OAI CI Virtual-Machine-based Test Environment: Testing an OAI variant</font></b>
    </td>
  </tr>
</table>

**Table of Contents**

[[_TOC_]]

# 1. Introduction #

Currently 1 build variant can be directly tested:

*  Physical Simulators

In addition, 2 build variants are used:

*  OAI eNB with ETHERNET transport
*  OAI UE with ETHERNET transport

for the following scenarios:

*  L1 simulator w/ a channel simulator (NOT IMPLEMENTED)
*  RF simulator : (IMPLEMENTED but not working as of 2019.w15)
*  L2 nFAPI simulator

Tests are run sequentially in the Jenkins pipeline because:

*  We want to mutualize the VM creation for an EPC
*  We have seen performance issues when running in parallel.


```bash
./ci-scripts/oai-ci-vm-tool test --help
OAI CI VM script
   Original Author: Raphael Defosseux
   Requirements:
     -- uvtool uvtool-libvirt apt-cacher
     -- xenial image already synced
   Default:
     -- eNB with USRP

Usage:
------
    oai-ci-vm-tool test [OPTIONS]

Options:
--------
    --job-name #### OR -jn ####
    Specify the name of the Jenkins job.

    --build-id #### OR -id ####
    Specify the build ID of the Jenkins job.

    --workspace #### OR -ws ####
    Specify the workspace.

 # OpenAirInterface Build Variants
    --variant enb-usrp        OR -v1     ( build and test  )
    --variant phy-sim         OR -v3     ( build and test  )
    --variant cppcheck        OR -v4     ( build and test  )
    --variant gnb-usrp        OR -v5     ( build and test  )
    --variant nr-ue-usrp      OR -v6     ( build and test  )
    --variant enb-ethernet    OR -v7     ( build and test  )
    --variant ue-ethernet     OR -v8     ( build and test  )
 # non-OSA Build Variants
    --variant flexran-rtc     OR -v10    ( build and test non-OSA )
 # OpenAirInterface Test Variants
    --variant l1-sim          OR -v20    ( test  )
    --variant rf-sim          OR -v21    ( test  )
    --variant l2-sim          OR -v22    ( test  )
    Specify the variant to build.

    --keep-vm-alive OR -k
    Keep the VM alive after the build.

    --help OR -h
    Print this help message.
```

# 2. Detailed Description #

Source file concerned: `ci-scripts/run_test_on_vm.sh`

**TBD when file is re-structured.**

# 3. Typical Usage #

## 3.1. Testing the physical simulators ##

```bash
$ ./ci-scripts/oai-ci-vm-tool test --workspace /var/jenkins/workspace/RAN-CI-develop --variant phy-sim --job-name RAN-CI-develop --build-id 47
12:54:29  ############################################################
12:54:29  OAI CI VM script
12:54:29  ############################################################
12:54:29  VM_NAME             = RAN-CI-develop-b47-phy-sim
12:54:29  VM_CMD_FILE         = RAN-CI-develop-b47-phy-sim_cmds.txt
12:54:29  JENKINS_WKSP        = /var/jenkins/workspace/RAN-CI-develop
12:54:29  ARCHIVES_LOC        = /var/jenkins/workspace/RAN-CI-develop/archives/phy_sim/test
12:54:29  ############################################################
12:54:29  Waiting for VM to be started
12:54:29  ############################################################
12:54:29  Warning: Permanently added '192.168.122.220' (ECDSA) to the list of known hosts.
12:54:30  RAN-CI-develop-b47-phy-sim has for IP addr = 192.168.122.220
...
13:04:48  Test Results are written to /home/ubuntu/tmp/cmake_targets/autotests/log/results_autotests.xml
13:05:00  ############################################################
13:05:00  Creating a tmp folder to store results and artifacts
13:05:00  ############################################################
13:05:00  /var/jenkins/workspace/RAN-CI-develop/archives/phy_sim/test /var/jenkins/workspace/RAN-CI-develop
13:05:04  /var/jenkins/workspace/RAN-CI-develop
13:05:04  ############################################################
13:05:04  Destroying VM
13:05:04  ############################################################
13:05:06  # Host 192.168.122.220 found: line 21
13:05:06  /home/eurecom/.ssh/known_hosts updated.
13:05:06  Original contents retained as /home/eurecom/.ssh/known_hosts.old
13:05:06  ############################################################
13:05:06  Checking run status
13:05:06  ############################################################
13:05:06  NB_FOUND_FILES = 1
13:05:06  NB_RUNS        = 20
13:05:06  NB_FAILURES    = 0
13:05:06  STATUS seems OK
```

Note that the VM instance is destroyed. You do that when you are sure your test is passing.

## 3.2. Test the RF simulator ##

```bash
./ci-scripts/oai-ci-vm-tool test --workspace /var/jenkins/workspace/RAN-CI-develop --variant rf-sim --job-name RAN-CI-develop --build-id 48 --keep-vm-alive
15:24:45  Currently RF-Simulator Testing is not implemented / enabled
15:24:45  Comment out these lines in ./ci-scripts/oai-ci-vm-tool if you want to run it
15:24:45  STATUS seems OK
```

## 3.3. Testing the L2-nFAPI simulator

```bash
./ci-scripts/oai-ci-vm-tool test --workspace /var/jenkins/workspace/RAN-CI-develop --variant l2-sim --job-name RAN-CI-develop --build-id 48
15:24:47  ############################################################
15:24:47  OAI CI VM script
15:24:47  ############################################################
15:24:47  ENB_VM_NAME         = RAN-CI-develop-b48-enb-ethernet
15:24:47  ENB_VM_CMD_FILE     = RAN-CI-develop-b48-enb-ethernet_cmds.txt
15:24:47  UE_VM_NAME          = RAN-CI-develop-b48-ue-ethernet
15:24:47  UE_VM_CMD_FILE      = RAN-CI-develop-b48-ue-ethernet_cmds.txt
15:24:47  JENKINS_WKSP        = /var/jenkins/workspace/RAN-CI-develop
15:24:47  ARCHIVES_LOC        = /var/jenkins/workspace/RAN-CI-develop/archives/l2_sim/test
15:24:47  ############################################################
15:24:47  Waiting for ENB VM to be started
15:24:47  ############################################################
15:24:47  Warning: Permanently added '192.168.122.110' (ECDSA) to the list of known hosts.
15:24:48  RAN-CI-develop-b48-enb-ethernet has for IP addr = 192.168.122.110
15:24:48  ############################################################
15:24:48  Waiting for UE VM to be started
15:24:48  ############################################################
15:24:49  Warning: Permanently added '192.168.122.90' (ECDSA) to the list of known hosts.
15:24:50  RAN-CI-develop-b48-ue-ethernet has for IP addr = 192.168.122.90
15:24:50  ############################################################
15:24:50  Test EPC on VM (RAN-CI-develop-b48-epc) will be using ltebox
15:24:50  ############################################################
15:24:50  EPC_VM_CMD_FILE     = RAN-CI-develop-b48-epc_cmds.txt
15:24:50  Waiting for VM to be started
15:24:50  Warning: Permanently added '192.168.122.156' (ECDSA) to the list of known hosts.
15:24:51  RAN-CI-develop-b48-epc has for IP addr = 192.168.122.156
...
15:42:44  ############################################################
15:42:44  Destroying VMs
15:42:44  ############################################################
15:42:46  # Host 192.168.122.110 found: line 18
15:42:46  /home/eurecom/.ssh/known_hosts updated.
15:42:46  Original contents retained as /home/eurecom/.ssh/known_hosts.old
15:42:47  # Host 192.168.122.90 found: line 18
15:42:47  /home/eurecom/.ssh/known_hosts updated.
15:42:47  Original contents retained as /home/eurecom/.ssh/known_hosts.old
15:42:47  ############################################################
15:42:47  Checking run status
15:42:47  ############################################################
15:42:47  STATUS failed?
```
---

Final step: [how to properly destroy all VM instances](./vm_based_simulator_destroy.md)

You can also go back to the [CI dev main page](./ci_dev_home.md)

