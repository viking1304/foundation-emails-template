# Foundation for Emails Template - UNOFFICIAL VERSION

### Updated on January 31st 2024

**Please open all issues with this `UNOFFICIAL VERSION` [here](https://github.com/viking1304/foundation-emails-template/issues) instead of the main Foundation for Emails repo.**

This is the unofficial starter project for [Foundation for Emails](https://get.foundation/emails.html), a framework for creating responsive HTML devices that work in any email client. It has a Gulp-powered build system with these features:

- Handlebars HTML templates with [Panini](http://github.com/zurb/panini)
- Simplified HTML email syntax with [Inky](http://github.com/zurb/inky)
- Sass compilation
- Image compression
- Built-in BrowserSync server
- Full email inlining process

## Reasons I made this unofficial version

Since my [pull request](https://github.com/foundation/foundation-emails-template/pulls) are not merged to the main repo after almost a year, I decided to continue my work independently of the main repo.

## What's new

- support for newer node versions
- basic support for [data-ogsb]/[data-ogsc] and u+ and div > u + prefixed styles
- basic dark mode support
- temporary removed css minification, to fix issues in some mail clients
- updated libraries

## Installation

To use this template, your computer needs [Node.js](https://nodejs.org/en/).    Recommended node version is `v18`, but some other node versions might work too.

*Node v10 support is totally dropped!*

### Using the CLI

`foundation-cli` is deprecated, so do not try to use it.

### Manual Setup

To manually set up the template, first download it with Git:

```bash
git clone https://github.com/viking1304/foundation-emails-template projectname
```

Then open the folder in your command line, and install the needed dependencies:

```bash
cd projectname
npm install
```

## Build Commands

Run `npm start` to kick off the build process. A new browser tab will open with a server pointing to your project files.

Run `npm run build` to inline your CSS into your HTML along with the rest of the build process.

Run `npm run litmus` to build as above, then submit to litmus for testing. *AWS S3 Account details required (config.json)*

Run `npm run mail` to build as above, then send to specified email address for testing. *SMTP server details required (config.json)*

Run `npm run zip` to build as above, then zip HTML and images for easy deployment to email marketing services. 

### Speeding Up Your Build

If you create a lot of emails, your build can start to slow down, as each build rebuilds all of the emails in the
repository. A simple way to keep it fast is to archive emails you no longer need by moving the pages into `src/pages/archive`.
You can also move images that are no longer needed into `src/assets/img/archive`. The build will ignore pages and images that
are inside the archive folder.

## Litmus Tests (config.json)

Testing in Litmus requires the images to be hosted publicly. The provided gulp task handles this by automating hosting to an AWS S3 account. Provide your Litmus and AWS S3 account details in the `example.config.json` and then rename to `config.json`. Litmus config, and `aws.url` are required, however if you follow the [aws-sdk suggestions](http://docs.aws.amazon.com/AWSJavaScriptSDK/guide/node-configuring.html) you don't need to supply the AWS credentials into this JSON.

```json
{
  "aws": {
    "region": "us-east-1",
    "accessKeyId": "YOUR_ACCOUNT_KEY",
    "secretAccessKey": "YOUR_ACCOUNT_SECRET",
    "params": {
        "Bucket": "elasticbeanstalk-us-east-1-THIS_IS_JUST_AN_EXAMPLE"
    },
    "url": "https://s3.amazonaws.com/elasticbeanstalk-us-east-1-THIS_IS_JUST_AN_EXAMPLE"
  },
  "litmus": {
    "username": "YOUR_LITMUS@EMAIL.com",
    "password": "YOUR_ACCOUNT_PASSWORD",
    "url": "https://YOUR_ACCOUNT.litmus.com",
    "applications": ["ol2003","ol2007","ol2010","ol2011","ol2013","chromegmailnew","chromeyahoo","appmail9","iphone5s","ipad","android4","androidgmailapp"]
  }
}
```

## Manual email tests (config.json)

Similar to the Litmus tests, you can have the emails sent to a specified email address. Just like with the Litmus tests, you will need to provide AWS S3 account details in `config.json`. You will also need to specify to details of an SMTP server. The email address to send to emails to can either by configured in the `package.json` file or added as a parameter like so: `npm run mail -- --to="example.com"`

```json
{
  "aws": {
    "region": "us-east-1",
    "accessKeyId": "YOUR_ACCOUNT_KEY",
    "secretAccessKey": "YOUR_ACCOUNT_SECRET",
    "params": {
        "Bucket": "elasticbeanstalk-us-east-1-THIS_IS_JUST_AN_EXAMPLE"
    },
    "url": "https://s3.amazonaws.com/elasticbeanstalk-us-east-1-THIS_IS_JUST_AN_EXAMPLE"
  },
  "mail": {
    "to": [
      "example@domain.com"
    ],
    "from": "Company name <info@company.com",
    "smtp": {
      "auth": {
        "user": "example@domain.com",
        "pass": "12345678"
      },
      "host": "smtp.domain.com",
      "secureConnection": true,
      "port": 465
    }
  }
}
```

For a full list of Litmus' supported test clients(applications) see their [client list](https://litmus.com/emails/clients.xml).

**Caution:** AWS Service Fees will result, however, are usually very low do to minimal traffic. Use at your own discretion.

## Outlook and Google specific styles

All Outlook and Google specific styles should be in the postcss-uncss ignore block or they will be removed as unused CSS.

```
// styles that should not be processed by uncss
// put [data-ogsb]/[data-ogsc] and u+ and div > u + prefixed styles here
/* uncss:ignore start */
[data-ogsb] table.twitter table td {
  background-color: #00629c !important;
  border-color: #00629c !important;
}

div > u + div .body .header {
  background-color: #666 !important;
}
/* uncss:ignore end */
```

**Known issues:** Do not use media queries in this block or they will appear twice in your HTML file.
