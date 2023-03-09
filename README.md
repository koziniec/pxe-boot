# PXE Booting lab OS image deployment system
An early work in progress and essentially a collection of progress notes

## Goals 
- DOCUMENT whatever this is and how to rebuild and configure it.
    - Seriously without documentation, in time any functionality will turn to rust.
- A Virtual Box, Arch Linux server VM to support lab deployment.
    - Why Arch?
        - The privililege or starting the project is power and I like Arch!
        - Packages very close to the authors DOCUMENTATION - Less confusion resulting from distro tweaks.
        - Stronger community skill set and high quality DOCUMENTATION with less noob noise.
    - Features, architecture and philosophy
        - It needs to be low stress
        - reliability and peace of mind over functionality
        - Keep everything in one VM - Monolythic
            - Standalone operation that is easily and quickly replicated for testing.
                - Encourages learning in a safe sandbox.
                - Develops familiarity with the restoration from a clone.
                - Accessible,Dev server can run on a local machine such as a laptop.
            - Confident backup and portability between servers
            - Components
                - DHCP Server
                - PXE server
                - NFS? repository of images

## Preparing the new lab
- Update values in /_config.yml
- put all images in: /assets/img
    - /assets/img/topology.png
- update copyright licence information in: /pages/about.md 
- Update lab title(s) here: /pages/index.md    (Don't change topology link)
- Put EVE topology files here: /_docs/eve   (initial.zip and final.zip)
- Build lab content here: /_docs/lab-exercise.md
