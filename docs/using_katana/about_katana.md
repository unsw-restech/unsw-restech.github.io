title: About Katana

Katana is a shared computational cluster located on campus at UNSW that has been designed to provide easy access to computational resources for groups working with non-sensitive data. It contains over 7,000 CPU cores, 25 GPU compute nodes (H200, GH200, A100, V100 and L40S), and 8PB of disk storage. Katana provides a flexible compute environment where users can run jobs that wouldn't be possible or practical on their desktop or laptop. For full details of the compute nodes including a full list see the compute node information section below.

Katana is powerful on its own, but can be seen as a training or development base before migrating up to systems like Australia's peak HPC system [Gadi at NCI](https://opus.nci.org.au/display/Help/0.+Welcome+to+Gadi). Research Technology Services also provide [training and support](https://unsw.sharepoint.com/sites/restech) for new users or those uncertain if High Performance Computing is the right fit for their research needs.

## System Configuration

- RPM based Linux OSes. Rocky 8 on the management plane and nodes
- OpenPBS version 23.06.06
- Large global scratch at `/srv/scratch`, local scratch at `$TMPDIR`
- 12, 48, 100, 200 hour [Walltime queues with prioritisation](/using_katana/running_jobs/#job-queue-limits-summary)

## Compute

- Heterogenous hardware: Dell, Lenovo, Supermicro, Xenon.
- More than 150 nodes

## GPU Compute

- 25 GPU nodes
    - 24 x Nvidia H200 141GB (6 nodes)
    - 1 x Nvidia GH200 480GB + 96GB (1 node)
    - 32 x Nvidia L40S 48GB (7 nodes)
    - 20 x Nvidia A100 40GB (3 nodes)
    - 32 x Nvidia V100 32GB (8 nodes)
- 12 GPU nodes have priority access for the school/group that purchased them
- 13 GPU nodes are for use by all researchers

!!! info
    Unfortunately, GPU nodes are in incredibly high demand. 
    We cannot provide special accommodation for any project. 
    You will need to wait in the common queue - or buy a GPU node for your group on which you will get priority

To access a GPU node interactively, you can use a command like

``` bash
[z1234567@katana ~]$ qsub -I -l select=1:ncpus=8:ngpus=1:mem=46gb,walltime=2:00:00
```

!!! info
    - You cannot use Tensorflow or Pytorch on the login nodes because they don't have GPUs. You will need to get access to a GPU node to do this. 
    - GPU-enabled software should be installed on a GPU node within an interactive session, in case the software is probing for GPU hardware or libraries.
    - Note the 2 hour limit - that is the *fastest* way to get onto the GPU nodes. Please check out [qsub command](/using_katana/running_jobs/#interactive-jobs) to find more options



