Originally published as [an article on LinkedIn](https://www.linkedin.com/pulse/crowdstrike-incident-biggest-outage-history-ali-raza-vpvrf/).

# Disclaimer

This article may have had error(s). Since publishing this, [the RCA](https://www.crowdstrike.com/blog/channel-file-291-rca-available/) of the incident is now available. It is requested to please read it for accurate and up-to-date information regarding incident-specific technicalities.

---

- [[#Introduction|Introduction]]
- [[#The Boot Process: From Hardware to Software|The Boot Process: From Hardware to Software]]
- [[#The Kernel: The Bridge Between Hardware and Software|The Kernel: The Bridge Between Hardware and Software]]
- [[#Kernel Drivers: The Kernel's Interaction with Hardware|Kernel Drivers: The Kernel's Interaction with Hardware]]
- [[#The CrowdStrike Incident: A Closer Look|The CrowdStrike Incident: A Closer Look]]
- [[#IMPACT|IMPACT]]

---

## Introduction

CrowdStrike is a cybersecurity IT organization that provides real-time protection services to enterprise customers, which are typically large companies with numerous computers. In essence, their service is like an anti-virus solution but on a much larger scale.

One crucial aspect to understand about anti-virus software is that malware, or harmful programs, can target a computer at any layer depending on where vulnerabilities exist, ranging from the motherboard of a personal computer (hardware) to applications like Google Chrome (software).

The CrowdStrike incident in July 2024 occurred "in the kernel space", which is the deepest layer of software in an Operating System (like Windows 10). A faulty kernel driver caused computers worldwide to become unusable **IF** they were using CrowdStrike's Falcon platform on a Windows operating system. To understand the resulting magnitude, it is astonishing to find out that CrowdStrike has more than 29,000 customers (companies) across 170 countries, according to Reuters.

You might be wondering: so, what was the incident? How did this happen? For this, we will need a trek across the booting process, something that your computer goes through every time it is restarted or shut down!

## The Boot Process: From Hardware to Software

When you plug in the power cord of a computer to the socket in your home, the silicon chips on the motherboard receive electric current, establishing a voltage, effectively "turning on" the machine. However, you as the user cannot do much with it until an Operating System (OS) like Windows 11 or macOS 'takes control' of the computer's hardware resources. Since the OS is itself a software, a 'bridge' must be established between the hardware and the OS when the computer turns on, so that you can use it.

The motherboard or main circuit board of your computer has a Central Processing Unit (CPU), which is by all means the "brain" of any computer. It contains a Program Counter that must keep 'ticking' to execute different instructions from memory. Well, it's actually the system clock that has to keep ticking in a literal sense, but the Program Counter directs the rest of the CPU about which instruction it should be working on. Normally, the Program Counter increments to read instructions from a set of binary instructions known as a software or computer program, but in our journey, the OS on which developers write software hasn't gained any control of the hardwrae yet. So, where does the Program Counter 'look' to find instructions for the CPU to work on?

The answer lies in the firmware, which is a set of binary instructions hardcoded into a small, non-volatile memory chip (permanent storage) on the main circuit board of the computer. You may have come across terms like Basic Input/Output System (BIOS) or UEFI -- these are examples of firmware! So, when we provide electrical power to the CPU, the Program Counter initializes to the address of the first instruction in the firmware, and keeps 'ticking' to execute all of the instructions contained in it. (Note that different computer architectures may invalidate this statement, but the way the concepts evolve logically remains principally true.)

The firmware instructions initiate the "booting" process, which includes basic validation testing of some critical chips on the motherboard to ensure they are fully functional before the OS takes control. It then locates the bootloader from all the devices interfaced with the CPU, and the bootloader finally gives control to the OS (they may be on the same permanent storage). Coming back to the firmware's testing, critical chips include memory chips, whose purpose is temporary storage (e.g., cache) or permanent storage (e.g., firmware). But memory is crucial in both hardware _and_ software contexts. In hardware, permanent memory has the firmware and bootloader. Whereas in software, the OS only truly has control when it can access memory, with the ability to read from and write to it -- but on your level, the user level, the OS only deals with virtual memory, not directly with the physical memory in hardware!

Although understanding this detail is crucial, it is not relevant for the CrowdStrike incident, so I will not address it. However, what _is_ important to understand is that to prevent the OS from having direct access to physical memory, the part of the OS closest to the hardware, which truly 'bridges' it with the hardware, holds the most significance.

## The Kernel: The Bridge Between Hardware and Software

The kernel is the most critical part of the OS, and comes into play during the boot process. After testing the hardware, the firmware locates the bootler and then points the Program Counter to it. In turn, among other things, the bootloader loads the kernel into Random Access Memory (RAM), which is temporary storage that requires continuous electrical power to retain its contents. The firmware is stored in a memory chip on the main circuit board of the computer, while the bootloader and OS are stored on an external memory chip/chips (HDD/SSD nowadays). External memory interfaces with the main circuit board through conducting wires, allowing the CPU physical access to it.

The kernel is the first thing the CPU initialises when it has 'found' the OS thanks to the bootloader. The kernel effectively handles all of the OS's interactions with the computer hardware, such as deciding which physical memories the OS can access and in what order contents can be written to memory. This brings us to the concept of privileges, essential in ascertaining precisely how critical the CrowdStrike incident was.

The kernel has the highest privilege level in the context of the OS because it is the only part of the OS that can directly interact with the computer hardware, in the form of binary instructions. It has to do this for the mapping between physical memory and virtual memory, as well as for interrupt handling. On the other end of the spectrum, user-facing software applications like Google Chrome (running on your OS) have a very low privilege level, meaning they are far removed from hardware-level instructions and cannot directly instruct the CPU to perform tasks like "point the Program Counter to _falana_ address in a physical memory".

The reason for this privilege disparity is that it is the kernel's responsibility to ensure that the rest of the OS can make use of all the hardware resources it requires for functioning satisfactorily. Without the kernel, your OS wouldn't be able to do anything even if it was loaded into memory! On the other hand, apps like Google Chrome are designed to work within the scope of the resources that are available to the OS to deliver functionality to the user. Granting hardware-level control to apps could lead to developers having complete control over your computer, which is undesirable from a security perspective, and is also why an OS shouldn't be designed without completely delegating hardware-level control to a kernel.

## Kernel Drivers: The Kernel's Interaction with Hardware

We've discussed the importance of the kernel, but how does it interact with hardware? As stated briefly earlier, it uses the same binary instructions the CPU is designed to understand, albeit this is done through software programs called kernel drivers. These drivers may be written in C or Assembly, but ultimately get compiled and/or assembled to (binary) machine code for the hardware to 'understand' anyway. We can think of it like this: just like the firmware tests hardware components to ensure they're operational, kernel drivers serve the purpose of 'triggering' specific hardware components, such as a keyboard -- a driver acts as a translator, ensuring that the binary data signals from the device hardware are correctly 'understood' by the OS.

CrowdStrike's Falcon platform services operate at the kernel level because malware can target any privilege level in the OS, and the most dangerous malware is the one that can exploit the kernel, granting it access to all the hardware. One possible way this can happen is if a maliciously designed device driver is installed on the OS, because you as the user don't read the driver code; let's say if this malicious driver is for the computer mouse you're using, it can cause left clicks to, well, do what it usually does, but _also_ do some other bad thing (like modifying something in memory to 'corrupt' your data). This is also why applications in the OS are not given a very high privilege level, because then there are more possible ways for malicious hackers to sneakily get malware on anyone's computer.

## The CrowdStrike Incident: A Closer Look

The CrowdStrike issue was specifically related to kernel drivers. An update was pushed out to all CrowdStrike Falcon customers, a component of CrowdStrike's security software, which is a regular and harmless process usually. However, this time, one of the drivers CrowdStrike had on all the customers' machines got corrupted by being filled with null bytes, making it an invalidly formatted driver that caused a null pointer access. Crucially, this issue only affected computers running Windows operating systems. Drivers are specific to kernels, meaning that a device driver written for the kernel of the Windows 10 OS will be exclusive to that kernel, since it's acting as a translator for _that_ kernel. (As an analogy, it's like different kernels have their own different 'language'.)

Kernel drivers like the one affected operate within the kernel space of the operating system, which is the most privileged area where core OS functions and critical device drivers run. When the OS attempts to load a corrupted driver during the boot process or when the driver is explicitly called to load, it can lead to system instability. In this case, when the OS tried to load the corrupted driver, it encountered an unrecoverable error, triggering a STOP error, commonly known as the Blue Screen of Death (BSOD) in Windows systems. This error occurs because the OS detected a critical issue that could potentially compromise system stability or security if operations were to continue.

The BSOD is a safety mechanism implemented by the operating system. Its purpose is to immediately halt all operations when a critical error is detected, preventing the OS from continuing to operate in an unstable or compromised state. This abrupt stop helps prevent potential data loss or further damage to the system that could occur if the OS continued to run with a severe error. While the failure of a single third-party driver like this one would not typically prevent basic OS functionality, drivers that operate at the kernel level can cause system-wide issues if they malfunction. This is because kernel-mode drivers have direct access to hardware and core system resources. Their failure can impact the stability and security of the entire system, which is why the OS responds so decisively to critical errors in this privileged space.

## IMPACT

The impact of this incident was significant, as CrowdStrike's customers are spread out worldwide, and thus operations were affected in places ranging from corporate offices to airports and banks! The BSOD caused by the invalid kernel driver rendered the affected computers unusable, and because the root cause files were in the kernel space, the only way to fix the computers is to manually boot them in safe mode and then either delete those files or update them (since CrowdStrike has now issued an update to fix the problem). Yes, manually means that you have to walk up to every computer and do certain steps!