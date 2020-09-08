# CKAD

CNCF CKAD prep material and resources

Preparation for CKAD for Kubernetes v1.18

Kubernetes versions:

- Kubernetes server: 1.18.4
- kubectl: 1.19

Contents:

- **[Imperative Commands](./imperative-commands.md)** - kubectl CLI commands to deploy resources and perform certain operations without creating a resource file.

Exercises:

To practice the imperative commands I highly recommend going through the exercises in the following repos. A good way to solve this exercise is to time yourself. You can either use a Stopwatch mode or Countdown mode. Whatever suits you :D

1. [dgkanatsios/CKAD-exercise](https://github.com/dgkanatsios/CKAD-exercises#contents): 

   Go through each category excercise atleast twice. 
   - First, without using any help i.e. __kubernetes.io docs__ or any __kubectl --help__ commands. This allows you to capture the gaps in your knowledge when it comes to imperative commands.
   - Second, time yourself and use only __kubectl --help__ and __kubectl explain__ commands. *kubectl --help* will get you the imperative command syntax. *kubectl explain* will help you identify correct fields when you encounter any syntax errors, especially while editing a YAML file. More advantages of kubectl explain in this [heptio blog](https://blog.heptio.com/kubectl-explain-heptioprotip-ee883992a243)


