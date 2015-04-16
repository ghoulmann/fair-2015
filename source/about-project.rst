======================
About this Project
======================

Concerns Addressed
==========================

* Not every student has access to a personal computing environment on which they can experiment with assitive technologies or productivity software, either because the resource are not available or because family members can be reluctant to give their Chelsea students permissive access to the technology.

* Assistive technology vendors market products with a one-size-fits-all philosophy, rather than offering each student a solution that meets her individual needs and preferences.

* Vendors work with schools to accustom students to proprietary technology and to produce customers loyal to their brands and products.

Our Challenge
==============

Can our team of student developers collaboratively create a computing environment that requires no more of a financial investment than $8, but that offers assistive, educational, and productivity software that suits every Chelsea School student’s individual learning needs and preferences?


Project Summary
================

The team will create a USB drive from which Chelsea School students can boot PCs at home from.

By booting from this flash drive, the students will have a broad collection of assistive, educational, and productivity software that will serve their individual learning needs preferences.

The software will run on an operating system optimized for smooth running -- even on older computers or computers with low resources.

By using this USB flash drive, students can have optimized work environments without affecting other family members' technology: no changes are made to the host computer or its data.

Project Objectives
===================

* Provide equitable access to resources for all students, including, but not restricted to, assistive and educational technologies -- not only at school, but also at home.

* Avoid one-size-fits-all approaches to assistive technology

* Provide a highly customizable desktop environment to suit individual preferences

* Provide a solution that requires minimal technical expertise or training

* Prepare students to anticipate and work fluently and *flexibly* with  multiple - often competing - computing environments found in post-secondary environments.

* Accomplish `Learning objectives <#learning-objectives>`__.


Learning Objectives
======================

The student will:

* install software packages from `repositories <https://en.wikipedia.org/wiki/Software_repository>`_

* add to `repositories <https://en.wikipedia.org/wiki/Software_repository>`_ available to the operating system

* differentiate system software from application software

* learn about different operating systems

* understand free software and open-source software licenses

* automate routine tasks by programming `shell scripts <#automated-package-installation>`_

Procedure
===========

1. Make an informed choice of a Ubuntu "flavor" that is best for this use case.[1]_

2. Research and note the compatible software applications that would be ideal for the students Chelsea School serves.

3. Research and record the install procedures for each selected application.

4. Create an `install script <#automated-package-installetion>`_ to automate as much of the application install process as possible.

5. Create a `virtual machine <what-vm.html>`_ with the chosen Ubuntu distribution as the operating system.

6. Install Filezilla so we can transfer our finished work from the virtual machine to a workstation.

7. Download distroshare-ubuntu to the virtual machine.

8. Upload and run the install script from step 4 to run the automated install process.

9. Install additional applications that could not be scripted, such as Lexia.

10. Use distro share-ubuntu to create a disk image based on the finalized configuration of the virtual machine. The automated install script will put this in the virtual machine with no extra work.

11. Ensure unetbootin is installed on the virtual machine with the disk image.

12. Use unetbootin to write the disk image to a USB drive.

13. Test the USB drive: insert in a computer that is powered down; turn on the computer; select boot from USB drive on power up.


Automated Package Installation
===============================

The following shell script - in the bash language - was written to automate the installation of customized software packages for the purpose of this project::

    #!/bin/bash -ex

    # Checks to make sure program is run by a root (privileged) user

    if [[ $EUID -ne 0 ]]; then
      echo "This script must be run as root"
      exit 1
    fi

    # downloads distroshare to tmp directory
    wget https://github.com/Distroshare/distroshare-ubuntu-imager/archive/master.zip -O /tmp/distroshare.zip
    # get from web public encryption key from the web | add the encryption key
    wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
    # shell script to write the google repository location in sources.list.d
    wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
    bash -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'
    # add repos for additional software
    add-apt-repository ppa:teejee2008/ppa
    add-apt-repository ppa:webupd8team/brackets
    add-apt-repository ppa:webupd8team/sublime-text-3
    add-apt-repository ppa:webupd8team/atom
    # Update software list to reflect these additions
    apt-get update

    # Install with no interaction all the following packages
    apt-get install -y conky-all \
        	atom \
        	sublime-text-installer \
        	brackets \
        	build-essential \
        	wine \
        	winetricks \
        	filezilla \
        	vim \
        	tmux \
        	cups-pdf \
        	writetype \
        	libreoffice \
        	tuxtype \
        	geany \
        	bluefish \
        	ipython \
        	idle \
        	python-setuptools \
        	gimp \
        	gimp-plugin-registry \
        	imagemagick \
        	kazam \
        	vlc \
        	lamp-server^ \
        	lubuntu-restricted-addons \
        	lubuntu-restricted-extras \
        	flashplugin-installer \
        	phpmyadmin \
        	google-chrome-stable \
        	espeak \
        	conky-manager  \
        	festival \
        	libreoffice \
        	unetbootin \
                git \
                zsh \
                mysql-workbench \
                cairo-dock \
                xcompmgr

    # Download Lexia to /opt/ for later installation
    wget http://c324714.r14.cf1.rackcdn.com/LexiaReading-9.0.0-b18-us.exe -O /opt/Lexia.exe

.. [1] "The difference between the flavors are in the set of packages installed. However, all flavors of Ubuntu use the same repository for downloading updates, so the same set of packages is available regardless of which flavor you have installed. New flavors have to go through a process to become a Recognized Flavor" (Ubuntu Wiki). Learn about available Ubuntu flavors `here <https://wiki.ubuntu.com/UbuntuFlavors>`_.

.. index:: bash, apt, virtual machine, shell script, free software, open source software, proprietary software, objectives
