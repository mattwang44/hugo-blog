---
title: "My Preparation of GCP ACE Certification (2020)"
subtitle: ""
description: "Learning materials for Google Cloud Platform (GCP) Associate Cloud Engineer (ACE) Certification Exam, including online courses, tutorial videos, notes, mock exams."
excerpt: "Learning materials for Google Cloud Platform (GCP) Associate Cloud Engineer (ACE) Certification Exam, including online courses, tutorial videos, notes, mock exams."
date: 2020-07-01
author: "Matt Wang"
image: "https://images.unsplash.com/photo-1562773534-b536e07e1a66?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80"
published: true
tags:
  - GCP
URL: "/2020/07/01/gcp-ace/"
categories: ["Tech"]
---

Go to [this article](/2020/06/15/gcp-ace-note/) for the note of the Coursera course [Cloud Engineering with GCP][coursera-ace]

[coursera-ace]: https://www.coursera.org/professional-certificates/cloud-engineering-gcp
[whizlabs-ace]: https://www.whizlabs.com/google-cloud-certified-associate-cloud-engineer/
[udemy-gcp-ace]: https://www.udemy.com/course/google-cloud-associate-cloud-engineer-certification/
[udemy-acg-ace]: https://www.udemy.com/course/google-certified-associate-cloud-engineer/
[la-ace]: https://linuxacademy.com/course/google-cloud-certified-associate-cloud-engineer/
[official-practice-exam]: https://cloud.google.com/certification/practice-exam/cloud-engineer
[braincert-mock]: https://www.braincert.com/course/16818-Google-Cloud-Certified-Associate-Cloud-Engineer-Practice-Exams
[free-qwiklab-code]: https://medium.com/@sathishvj/qwiklabs-free-codes-gcp-and-aws-e40f3855ffdb
[bad-bad-whizlabs]: https://acloud.guru/forums/gcp-certified-associate-cloud-engineer/discussion/-LrSl-HrhjbJa95KzcZf/Which%20Exam%20simulator~2FPractice%20exam%20would%20be%20the%20closest%20to%20an%20actual%20exam%3F

After sailing through the storm with my company (thank you COVID-19 :dizzy_face:),
I decided to have a two-week off studying GCP ACE exam on my own and leave all the things behind.
I've passed the exam in late June of 2020 and got the [certification](https://www.credential.net/6ed46944-6ab9-4f87-b19e-eea0d4a5517b?key=6f4f5f74408e1291fc997d8842f33f144a89ec669e204a3567c18d4566a4a0bb).
This article records how I prepare for the exam and those awesome reading materials I've found.

## Preparation Materials

During my preparation of GCP ACE Exam, I spent most of time on these materials:

- **[Course: Cloud Engineering with GCP][coursera-ace]**:

  This specialization, which is recommended by GCP officially and many examinees,
  contains four courses that provide the necessary knowledge and one course briefly
  introduce most of the topics listed in the official exam guide.
  Before taking the exam, I watched all the video sessions twice,
  ran through all the hands-on labs at least once, and passed all the quiz
  (to get the course certificate). However, it does not cover much about GKE,
  networking, and IAM related knowledge. You may need to spend more time on
  the documents of these topics or hand-on labs on [Qwiklabs](https://google.qwiklabs.com/).

- **[Qwiklabs](https://google.qwiklabs.com/)**:

  The labs embedded in Coursera course are actually hosted by Qwiklabs,
  so if you want more hand-on practices without adding too much expense, Qwiklabs is your best choice.
  I started to learn about GCP in late 2018 and have completed [several quests](https://google.qwiklabs.com/public_profiles/b323116d-a497-4a5e-bc95-f14a1842f059)
  before I decided to take the exam. If you're new to Qwiklabs, you can get the
  [free one-month-pass](https://go.qwiklabs.com/cloud-study-jams-2020) if you finish specific quests.
  Besides, Qwiklabs releases free credits from time-to-time and you can find the redeem codes from
  Qwiklabs' [Facebook](https://www.facebook.com/qwiklabs/) or [Twitter](https://twitter.com/qwiklabs?lang=en)
  or just go to [sathishvj's post][free-qwiklab-code].

- **[Whizlabs' ACE Mock Exams (160Q)][whizlabs-ace]**:

  I found several mock exam providers and randomly chose Whizlabs after finishing its free 25 questions.
  In my opinion, it's a bit harder than the actual exam.
  I failed all three mock exams when I first took them,
  but learned a lot from searching the answer of each question.
  However, I found [this post][bad-bad-whizlabs] after taking the exam saying that Whizlabs might steal the questions from official exam.
  I might choose other providers if I found the post before the exam.

- **[Udemy: Ultimate ACE 2020][udemy-gcp-ace]**:

  Before taking the exam, I discovered that I bought this on Udemy more than one year ago but forgot it since then.
  I skipped the courses and took the practice exam only.
  Although it's slightly easier than the actual exam, I think it's well-designed and could be a helpful resource if I had more time on it.

So, it costed me about \$140 to get the ACE certification:

| Item                                                 | Price | Note                                                                |
| :--------------------------------------------------- | :---- | :------------------------------------------------------------------ |
| Exam registration                                    | \$70  | Normally it's \$125 but somehow it's cheaper in Taiwan              |
| [Coursera: Cloud Engineering with GCP][coursera-ace] | \$49  | Note that GCP releases free one-month-pass a couple times in a year |
| [Whizlabs: Mock Exams][whizlabs-ace]                 | \$10  | \$19.95 on the price tag but currently 50% off due to COVID-19      |
| [Udemy: Ultimate ACE 2020][udemy-gcp-ace]            | \$10  | I bought it long ago when it's on sale                              |

## Useful Materials

### About ACE Certification

- [Exam Overview](https://cloud.google.com/certification/cloud-engineer)
- [Official Exam Guide](https://cloud.google.com/certification/guides/cloud-engineer)
- [Google Cloud Training](https://cloud.google.com/training)

### Online Courses & Mock Exams

|                                        Name                                        |      Platform       |              Price              | Course | Mock Exam |                                                                                     |
| :--------------------------------------------------------------------------------: | :-----------------: | :-----------------------------: | :----: | :-------: | :---------------------------------------------------------------------------------: |
|    [Cloud Engineering with Google Cloud Professional Certificate][coursera-ace]    |      Coursera       |           \$50/month            |   Y    |     N     |                     Recommended (but it doesn't cover GKE much)                     |
|             [Google Cloud Certified Associate Cloud Engineer][la-ace]              |    LinuxAcademy     |              \$50               |   Y    |  Y (50Q)  |                          Recommended by many Reddit users                           |
|      [Ultimate Google Certified Associate Cloud Engineer 2020][udemy-gcp-ace]      |     GCP @Udemy      |              ~\$60              |   Y    |  Y (70Q)  |                               Mostly on sale (<\$15)                                |
|      [Google Certified Associate Cloud Engineer Certification][udemy-acg-ace]      | A Cloud Guru @Udemy |             ~\$230              |   Y    | Y (100Q)  |                               Mostly on sale (<\$20)                                |
|                  [Official Practice Exam][official-practice-exam]                  |    Google Forms     |              free               |   N    |  Y (25Q)  |                     Too easy but make sure you run through this                     |
|          [Google Cloud Certified Associate Cloud Engineer][whizlabs-ace]           |      Whizlabs       | \$9.95 (course) /\$19.95 (exam) |   Y    | Y (160Q)  |                   It's mock exam is a bit harder than actual exam                   |
| [Google Cloud Certified - Associate Cloud Engineer Practice Exams][braincert-mock] |      BrainCert      |              \$30               |   N    | Y (250Q)  |                         Didn't buy it so difficulty unknown                         |
|               [Hand-on Labs@Qwiklabs](https://google.qwiklabs.com/)                |      Qwiklabs       |        Charge by credit         |   N    |     N     | For hand-on labs. [Free credits][free-qwiklab-code] are available from time to time |

### Study Materials

#### Well-known Experts

|                 Name                 |                                                             Repo                                                              |                        Medium                        |                                        YouTube                                         |                        Course                        |
| :----------------------------------: | :---------------------------------------------------------------------------------------------------------------------------: | :--------------------------------------------------: | :------------------------------------------------------------------------------------: | :--------------------------------------------------: |
|              sathishvj               | [awesome-gcp-certifications](https://github.com/sathishvj/awesome-gcp-certifications/blob/master/associate-cloud-engineer.md) |     [sathish vj](https://medium.com/@sathishvj)      | [AwesomeGCP](https://www.youtube.com/playlist?list=PLQMsfKRZZviRwqJwNmh1eAWnRMvlrk40x) |                          -                           |
| Joseph Holbrook (The Cloud Tech Guy) |                                                               -                                                               | [Joseph Holbrook](https://medium.com/@jholbrook2015) |   [The Cloud Tech Guy Joe](https://www.youtube.com/channel/UCLcRBsiL_BIgDdn7P6uZbnQ)   | [Udemy](https://www.udemy.com/user/joseph-holbrook/) |

#### Other

- [ddneves/awesome-gcp-certifications (outdated)](https://github.com/ddneves/awesome-gcp-certifications#Google-Cloud---Associate-Cloud-Engineer)
- [Whizlabs' ACE cheatsheet](https://www.whizlabs.com/blog/gcp-cheat-sheet/)
- [Viniciuscoding/granja_Google_Associate_Cloud_Engineer](https://github.com/Viniciuscoding/granja_Google_Associate_Cloud_Engineer)
- [WebLeash/gcp_associate_engineer](https://github.com/WebLeash/gcp_associate_engineer/blob/master/gcloud_cheat_sheet.md)
- [TiagoGouvea/google-cloud-associate-cloud-engineer-preparation](https://github.com/TiagoGouvea/google-cloud-associate-cloud-engineer-preparation)
- [DRpandaMD/gcp-certified-associate-cloud-engineer-2020](https://github.com/DRpandaMD/gcp-certified-associate-cloud-engineer-2020)
- [Shekhar Sarker: Notes from my Google Cloud Certification Exam](https://medium.com/@shekharsarker84/notes-from-my-google-cloud-certification-exam-9ac056105121)
- [Charles.J@medium](https://medium.com/@charles_j/how-i-passed-the-google-cloud-associate-engineer-certification-63a0fd932057): [ACE Study Note](https://docs.google.com/document/d/1u6pXBiGMYj7ZLBN21x6jap11rG6gWk7n210hNnUzrkI/edit)
