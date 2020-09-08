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


1. [bmuschko/ckad-prep](https://github.com/bmuschko/ckad-prep#demos)

    This repo also has some good set of scenario based questions, where you perform a sequence of operations on a same set of resources. It helps in linking different concepts to solve more practical problems.

    Time your exercise when practicing these scenarios.

1. [bbachi/CKAD-Practice-Questions](https://github.com/bbachi/CKAD-Practice-Questions#table-of-contents)

     This repo also offers a variety of practice questions to get you started and brush up your *kubectl* skills. 

     Most of the questions here can be covered using the kubectl commands. There are very few questions which need additional configuration after you create a resource or use `--dry-run` option to add those configuration fields.

     The same questions are available in a [Medium Blog](https://medium.com/bb-tutorials-and-thoughts/practice-enough-with-these-questions-for-the-ckad-exam-2f42d1228552) by same author as this repo. 
