# CMPE283 Assignment 2 & Assignment 3


<h3>Work done by Abhinav (016001336):</h3>
We began working on the assignment 02 once our assignment 01 environment was built successfully. We decided to edit the cpuid.c and vmx.c at first and then rebuild the kernel. I edited the cpuid.c and added if..else condition in the kvm_emulate_cpuid block code and commited it to the GitHub repo inside /arch/x86/kvm. After Akshara, commited the changes she made to vmx.c, I rebuilt the kernel and rebooted the VM. I also installed KVM on the hypervisor so that a guest VM can be created and test script can be executed on that.  <br>
  
<h3>Work done by Akshara (016006003):</h3>
I worked with Abhinav for this assignment. On my machine, I edited the vmx.c file according to the edits made in cpuid.c by Abhinav. I pushed the changes to GitHub. I then tried to rebuild the kernel.There were some errors encountered while rebuilding the kernel so we decided to start from scratch for assignment 02. After rebuilding the kernel, I also installed gnome on ubuntu host so that we could work with GUI and work on virtual machine manager. I then worked with Abhinav to help create the nested VM.  <br>

<h3>Steps followed:</h3>
  
1. Update the files arch/x86/kvm/vmx/vmx.c and arch/x86/kvm/cpuid.c <br>
2. Rebuild the kernel using ```make modules``` command and ```make INSTALL_MOD_STRIP=1 modules_install && make install```.<br>
3. Run ```lsmod | grep kvm``` to check if the kvm modules are preloaded.<br>
4. If they are already present remove them using ```rmmod kvm``` and ```rmmod kvm_intel``` commands.<br>
5. Run ```modprobe kvm``` and ```modprobe kvm_intel``` commands to reload edited kvm modules.<br>
  
6. Optional- We installed GUI for out Ubuntu host for a friendly UI and ease fo work. Also, we wanted to use Virtual machine manager to created the nested VM. GUI can be enabled on Ubuntu VM using below commands:(<a href="https://subscription.packtpub.com/book/big-data-and-business-intelligence/9781788474221/1/ch01lvl1sec15/installing-and-configuring-ubuntu-desktop-for-google-cloud-platform">ref</a>)<br>
  ```$ sudo apt-get install gnome-shell``` <br>
  ```$ sudo apt-get install ubuntu-gnome-desktop``` <br>
  ```$ sudo apt-get install autocutsel``` <br>
  ```$ sudo apt-get install gnome-core``` <br>
  ```$ sudo apt-get install gnome-panel``` <br>
  ```$ sudo apt-get install gnome-themes-standard``` <br> 
  
7. Install xrdp through the below commands- we need this to be able to access Ubuntu in GUI mode.<br>
  ```sudo apt-get update``` <br>
  ```sudo apt-get install -y xrdp``` <br>
  ```sudo apt-get install -y xfce4``` <br>
  ```sudo service xrdp restart``` <br>

8. Login to the host VM using RDP to have graphical interface.<br> 
9. To enable the kvm module in the host and install necessary packages, run the below commands from host terminal: (<a href="https://www.tecmint.com/install-kvm-on-ubuntu/">ref</a>) <br>
  ```sudo apt install qemu qemu-kvm qemu-system qemu-utils``` <br>
  ```sudo apt install libvirt-clients libvirt-daemon-system virtinst``` 
10. Run Virtual Machine Manager and create a new VM inside the host. (download the iso file or guest VM as a prerequisite)<br>
11. Install Guest OS once the VM is created and login to the nested VM.<br>
12. Install CPUID using ``` sudo apt install cpuid ``` if it is an Ubuntu VM <br>  
13. Run the command ```cpuid -l 0x4FFFFFFF``` to verify the output.<br>
14. Run the test bash script to produce results and print number of exits.<br>
15. Run the test2 bash script to produce number of cycles in ebx and ecx registers when eax=0x4ffffffe.<br>



<h3>Output Screenshots:</h3>
<ul>
<li>Output screen that verifies that kvm is installed on Ubuntu host.<br>

![Image 1](https://user-images.githubusercontent.com/99863530/205812527-1d959c48-b96a-4e66-957b-670a29fde9d5.png)
<br>
  
<li>Output screen that shows nested VM created on KVM Host:<br>
  
  ![image 2](https://user-images.githubusercontent.com/99863530/205812989-24162ae1-4222-4bd6-9a38-bb644af0f80a.png)
<br>
  
<li>Output screen that shows number of exits when eax=0x4fffffff:<br>
  
  ![12345](https://user-images.githubusercontent.com/99863530/205814357-055b34f1-7946-4df4-990e-eaecfa15a135.png)
  <br>
    
<li>Output screen that shows cycles spent when eax=0x4ffffffe:<br>

  ![Final](https://user-images.githubusercontent.com/99863530/205815252-21b6055f-0707-4eca-8702-4bda325493a4.PNG)


Assignment 3: Instrumentation Via Hyper-call II (Add New CPUID Emulation Features in KVM)
This assignment (A3) is to modify the CPUID emulation code in KVM to report back additional information when special CPUID leaf nodes are requested:

  For CPUID leaf node %eax=0x4FFFFFFE:

  Return the number of exits for the exit number provided (on input) in %ecx
  This value should be returned in %eax
  For CPUID leaf node %eax=0x4FFFFFFF:

  Return the time spent processing the exit number provided (on input) in %ecx
  Return the high 32 bits of the total time spent for that exit in %ebx
  Return the low 32 bits of the total time spent for that exit in %ecx
  At a high level, you will need to perform the following:

    Start with your assignment 2 environment

  Modify the kernel code with the assignment(s) functionality:

  Determine where to place the measurement code (for exit counts and # cycles)
  Create new CPUID leaf 0x4FFFFFFE, 0x4FFFFFFF
  Report back information as described above
  Create a user-mode program that performs various CPUID instructions required to test your assignment

Pro-tip: This can be achieved on ubuntu by installing the ‘cpuid’ package
Run this user mode program in the inner VM
There is no need to insmod anything like assignment 1 did
Verify proper output


Assignment 3 Results as below:
  
1. GCP-VM-L2-A3-Test-Result-E-0
  
  ![s1](https://user-images.githubusercontent.com/99863530/207123969-86dfce7f-2fb9-4596-bf63-392356310463.jpg)
  
2. GCP-VM-L2-A3-Test-Result-F-0
  
  ![s2](https://user-images.githubusercontent.com/99863530/207124785-aed2c639-d0f9-4492-b60f-9fdb346bb249.jpg)

3. VM-L2-A3-Test-Result-Statics-0
  
  ![s3](https://user-images.githubusercontent.com/99863530/207125054-dae3c65d-b0bd-4c67-b45a-e65c3482af89.jpg)

  
GCP-VM-L1-Demsg-For-A3-Test-E-0
  
  ![s4](https://user-images.githubusercontent.com/99863530/207125458-9ea26ba4-ca48-4066-8aed-ce978081d4ab.jpg)
  
GCP-VM-L1-Demsg-For-A3-Test-F-0
  
  ![s5](https://user-images.githubusercontent.com/99863530/207125639-6c40f565-555b-45c2-8544-68c3563ac013.jpg)
  
Assignment 3 Questions:

Does the number of exits increase at a stable rate?

    No, some of the exits increase, some of them stay the same. Among the ones that increase, they have different increment ratio depends on the exit type. 
  
Are there more exits performed during certain VM operations?

    Yes, for example, EXIT_REASON_CPUID(10), EXIT_REASON_IO_INSTRUCTION(30), EXIT_REASON_MSR_READ(31), etc.
  
Approximately how many exits does a full VM boot entail?

      It entails total 912772 exits with total 13080154698 cycle times.
  
Of the exit types defined in the SDM, which are the most frequent? Least?

    EXIT_REASON_EPT_VIOLATION(48) happens the most frequent, EXIT_REASON_DR_ACCESS(29) happens the least frequent
    Also, EXIT_REASON_MSR_READ(31) has the highest increment over time, EXIT_REASON_EPT_VIOLATION(48) has the lowest increment over time.


