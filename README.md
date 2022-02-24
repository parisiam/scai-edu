# SCAI Education website (Moodle)

SCAI Education is a website prepared by [Parisiam](https://parisiam.com) for [SCAI](http://scai.sorbonne-universite.fr) : it presents a series of courses in the Artificial Intelligence field and uses [Moodle](https://moodle.org) as LMS.

This repository is used to share Moodle resources with SCAI team and it contains:

- User manuals
- Documentation
- HTML templates and snippets

The address of the website is https://scai-education.sorbonne-universite.fr and a demo version of the Moodle is available at https://moodle.parisiam.com. Checkout course and lesson summary [here](https://docs.google.com/spreadsheets/d/1RZl1t4MYZvq9OzZsDOilMF0pLLdg5-bpXrxMy14zskc).

# User manuals

- [Managing workshops](manuals/workshop.md)

# Documentation

## Functionnalities and specifications

This section explains what choices were made for the settings.

- [Badges](docs/badge.md)
- [Certificates](docs/certificate.md)
- [Quiz](docs/quiz.md)
- [Glossary](docs/glossary.md)
- [Course duration](docs/course_duration.md)
- [Obtaining badge & certificate](docs/obtaining_badge_certif.md)
- [Tags](docs/tags.md)
- [Workshop (essay)](docs/workshop.md)
- [Front page](docs/frontpage.md)
- [Registration form](docs/registration.md)
- [Categories](docs/categories.md)

## Installation of Moodle

The version of Moodle used is 3.11.

- [Using Git to install and update Moodle](docs/git.md)
- [Multilingual preparation](docs/multilingual.md)
- [List of additional plugins required](docs/plugins.md)

## Graphic design

- [Design specifications](docs/graphic_design.md)

## Server specifications

- PHP 7.4 or above compatible with Moodle 3.11
- Mysql 5.7
- Apache 2 server

# Moodle testing website

Moodle offers an online test version available at https://school.moodledemo.net/. It is reset every hour. You may connect as an **manager**, a **teacher** or a **student** and test every aspect of Moodle.

- Connect as a student with the username **student** and password **moodle**.
- Connect as a teacher with the username **teacher** and password **moodle**.
- Connect as a manager as with the username **manager** and password **moodle**.

To simulate a scenario with Moodle you may need to use other accounts. You can download a list [here](docs/mount_orange_other_accounts.pdf). Basically, just use the first part the email (before @) of the user as username, and use moodle as password.

# Inspirations

- Interesting presentation of AI course references on this website: https://eiaschum.ca/en/trainings
- A Moodle website that does not look like Moodle: https://mooc.tela-botanica.org
- Not a Moodle but a nice touch and good content organisation: https://www.open.edu/openlearn/
- An AI site with which SCAI is involved and that was an inspiration in terms of design: https://www.elementsofai.com
