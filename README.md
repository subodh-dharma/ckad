# CKAD

CNCF CKAD prep material and resources

Preparation for CKAD for Kubernetes v1.19

Kubernetes versions:

- Kubernetes server: 1.19
- kubectl: 1.19

Contents:

- **[Imperative Commands](./imperative-commands.md)** - kubectl CLI commands to deploy resources and perform certain operations without creating a resource file.
- **[Rapid Fire](./rapid-practice-tests.md)** - a list of problem statements to solve the problems rapidly. This list of problems will help you check your speed for executing simple tasks. Most of the tasks can be achieved using imperative commands. Some questions are interdependent. I would recommend to solve questions in the given order. Allocate a maximum of 1.5 minutes to each question.

Courses:

Here are some online courses you can enroll in to get started with the preparation for CKAD exams.

1. [Udemy - Mumshad Mannambeth](https://www.udemy.com/course/certified-kubernetes-application-developer/)

Online Platforms:

I came across a couple of online platforms that can be used to spin up the latest Kubernetes cluster for your practice.

1. [kodekloud](https://kodekloud.com/) - If you enroll in Udemy's course listed above, you will get free access to this platform. This platform has scenarios classified based on various sections and concepts required for the exam.

1. [Game of PODs](https://kodekloud.com/p/game-of-pods) - This is a fun way to practice your exam preparation skills. This platform gamifies the Kubernetes concepts, problems, and scenarios to help you strengthen your skills and understanding of various concepts. This one is completely **FREE!!** I would highly recommend to go through this at least once. I am sure you will enjoy it!

1. [killer.sh](https://killer.sh/) - This is a CKAD/CKS/CKA exam simulator. This platform offers two sessions that last for 36 hours each. You are free to use the platform for your practice once you finish the mock exam. Currently 1 time activation (2 sessions) is charged for $35.00 (USD).

Exercises:

To practice the imperative commands, I highly recommend going through the exercises in the following repos. The best approach to solve this exercise is to time yourself. You can either use a Stopwatch mode or Countdown mode. Whatever suits you :D

1. [dgkanatsios/CKAD-exercise](https://github.com/dgkanatsios/CKAD-exercises#contents):

   Go through each category exercise at least twice.
   - First, without using any help like  __kubernetes.io docs__ or any __kubectl --help__ commands. It allows you to capture the gaps in your knowledge when it comes to imperative commands.
   - Second, time yourself and use only __kubectl --help__ and __kubectl explain__ commands. *kubectl --help* will get you the imperative command syntax. *kubectl explain* will help you identify correct fields when you encounter any syntax errors, especially while editing a YAML file. More advantages of kubectl explain in this [heptio blog](https://blog.heptio.com/kubectl-explain-heptioprotip-ee883992a243)

1. [bmuschko/ckad-prep](https://github.com/bmuschko/ckad-prep#demos)

    This repo also has a good set of scenario-based questions, where you perform a sequence of operations on the same resources. It helps in linking different concepts to solve more practical problems.

    Time your exercise when practicing these scenarios.

1. [bbachi/CKAD-Practice-Questions](https://github.com/bbachi/CKAD-Practice-Questions#table-of-contents)

     This repo also offers a variety of practice questions to get you started and brush up on your *kubectl* skills.

     Most of the questions here can be covered using the kubectl commands. Some questions need additional configuration after you create a resource.

     The same questions are available in a [Medium Blog](https://medium.com/bb-tutorials-and-thoughts/practice-enough-with-these-questions-for-the-ckad-exam-2f42d1228552) by the same author as this repo.

1. [zealvora/certified-kubernetes-application-developer](https://github.com/zealvora/certified-kubernetes-application-developer/tree/master/practice-tests)

      This repo offers some basic questions to practice Kubernetes skills required for CKAD exam prep. You can go through this set of questions to get the gist of the types of questions. Although all the questions aren't as challenging as one can expect but they cover all the sections in the syllabus.
