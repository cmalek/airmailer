.. module:: ldap_faker
  :noindex:

=========
Airmailer
=========

Current version is |release|.

This package provides a facade class for sending e-mail messages using either
straight SMTP or via AWS SES.

Installation
============

To install from PyPI::

   pip install airmailer

Usage: SMTP
-----------

.. code:: python

    from airmailer.backend import SMTPEmailBackend

    backend = SMTPEmailBackend(
        'smtp.example.com',
        username='example',
        password='password'
        use_tls=True
    )
    num_sent = backend.send_mail(
        'Example Subject',
        'Here is my message',
        'from@example.com',
        ['to@destination.com'],
        html_message='<p>here is my HTML message</p>'
    )


Usage: SES
----------

AWS IAM Policy
**************

Create an IAM Policy like so, and attach it to your EC2 instance profile, ECS task role, etc.:

.. code:: javascript

   {
      "Version": "2012-10-17",
      "Statement": [
         {
            "Effect": "Allow",
            "Action": ["ses:SendRawEmail"],
            "Resource":"*"
         }
      ]
   }

You can invoke the :py:class:`SESEmailBackend` with any AWS credential source
that boto3 can use.  See `Boto3 Configuration Guide
<https://boto3.amazonaws.com/v1/documentation/api/latest/guide/configuration.html>`_
for details.


Explicit credentials
^^^^^^^^^^^^^^^^^^^^

.. code:: python

    from airmailer.backend import SESEmailBackend

    backend = SESEmailBackend(
        aws_access_key_id="__THE_ACCESS_KEY_ID__",
        aws_secret_access_key="__THE_SECRET_ACCESS_KEY__",
        aws_region_name="__THE_REGION_NAME__",
        configuration_set_name='example'
    )
    num_sent = backend.send_mail(
        'Example Subject',
        'Here is my message',
        'from@example.com',
        ['to@destination.com'],
        html_message='<p>here is my HTML message</p>'
    )

or

.. code:: python

    from airmailer.backend import SESEmailBackend
    from botocore.config import Config

    my_config = Config(
        aws_access_key_id="__THE_ACCESS_KEY_ID__",
        aws_secret_access_key="__THE_SECRET_ACCESS_KEY__",
        region_name="__THE_REGION_NAME__",
    )

    backend = SESEmailBackend(
        aws_config=my_config,
        configuration_set_name='example'
    )

Credentials from the environment
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Configure your credentials in any way named in the `Boto3 Configuration Guide
<https://boto3.amazonaws.com/v1/documentation/api/latest/guide/configuration.html>`_.
This includes AWS specific environment variables, shared credential files, and
IAM roles from metadata endpoints.

Then:

.. code:: python

    from airmailer.backend import SESEmailBackend

    backend = SESEmailBackend(configuration_set_name='example')
    num_sent = backend.send_mail(
        'Example Subject',
        'Here is my message',
        'from@example.com',
        ['to@destination.com'],
        html_message='<p>here is my HTML message</p>'
    )


.. toctree::
   :maxdepth: 2

   api