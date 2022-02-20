<h1>BADGE</h1>

[TOC]

# Content of the certificate

![image-20220219212004944](.img/certificate/image-20220219212004944.png)

# Plugins to install

The certificate/diploma consists in **two plugins**:

- https://moodle.org/plugins/tool_certificate (/admin/tool/certificate) to generate the certificate template.
- https://moodle.org/plugins/mod_coursecertificate (/mod/coursecertificate) to emit the certificate. The certificate is added as an **activity** to a course.

# About security

The certificate can be secured through several mechanisms that can be simultaneously applied: **code, QR code, digital signature**.

The digital signature works through a `.ctr` file. Se: https://docs.moodle.org/311/en/Certificate_templates

>  ATTENTION to regenerate a proper certificate on the production server.

# Resources

- https://docs.moodle.org/311/en/Certificates
- https://docs.moodle.org/311/en/Course_certificate_activity
- https://youtu.be/WQeL9mQJMj0

# Examples of certificates

![image3](.img/certificate/image3.jpg)

![image10](.img/certificate/image10.jpg)
