Platform

# Set Up SSO with Azure AD

* * *

This feature is only available on the Team and Enterprise Plans. Contact [Sales](https://forms.supabase.com/enterprise) before doing these steps.

Looking for docs on how to add Single Sign-On support in your Supabase project? Head on over to [Single Sign-On with SAML 2.0 for Projects](https://supabase.com/docs/guides/auth/enterprise-sso/auth-sso-saml).

Supabase supports single sign-on (SSO) using Microsoft Azure AD.

## Step 1: Add and register an Enterprise application [\#](https://supabase.com/docs/guides/platform/sso/azure\#add-and-register-enterprise-application)

Open up the [Azure Active Directory](https://portal.azure.com/#view/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/~/Overview) dashboard for your Azure account.

Click the _Add_ button then _Enterprise application_.

![Azure AD console: Default Directory Overview](https://supabase.com/docs/img/sso-azure-step-01.png)

## Step 2: Choose to create your own application [\#](https://supabase.com/docs/guides/platform/sso/azure\#create-application)

You'll be using the custom enterprise application setup for Supabase.

![Azure AD console: Browse Azure AD Gallery, select: Create your own application](https://supabase.com/docs/img/sso-azure-step-02.png)

## Step 3: Fill in application details [\#](https://supabase.com/docs/guides/platform/sso/azure\#add-application-details)

In the modal titled _Create your own application_, enter a display name for Supabase. This is the name your Azure AD users see when signing in to Supabase from Azure. `Supabase` works in most cases.

Make sure to choose the third option: _Integrate any other application you_
_don't find in the gallery (Non-gallery)_.

![Azure AD console: Create your own application modal](https://supabase.com/docs/img/sso-azure-step-03.png)

## Step 4: Set up single sign-on [\#](https://supabase.com/docs/guides/platform/sso/azure\#set-up-single-sign-on)

Before you get to assigning users and groups, which would allow accounts in Azure AD to access Supabase, you need to configure the SAML details that allows Supabase to accept sign in requests from Azure AD.

![Azure AD console: Supabase custom enterprise application, selected Set up single sign-on](https://supabase.com/docs/img/sso-azure-step-04.png)

## Step 5: Select SAML single sign-on method [\#](https://supabase.com/docs/guides/platform/sso/azure\#saml-sso)

Supabase only supports the SAML 2.0 protocol for Single Sign-On, which is an industry standard.

![Azure AD console: Supabase application, Single sign-on configuration screen, selected SAML](https://supabase.com/docs/img/sso-azure-step-05.png)

## Step 6: Upload SAML-based sign-on metadata file [\#](https://supabase.com/docs/guides/platform/sso/azure\#upload-saml-metadata)

First you need to download Supabase's SAML metadata file. Click the button below to initiate a download of the file.

[Download Supabase SAML Metadata File](https://alt.supabase.io/auth/v1/sso/saml/metadata?download=true)

Alternatively, visit this page to initiate a download: `https://alt.supabase.io/auth/v1/sso/saml/metadata?download=true`

Click on the _Upload metadata file_ option in the toolbar and select the file you just downloaded.

![Azure AD console: Supabase application, SAML-based Sign-on screen, selected Upload metadata file button](https://supabase.com/docs/img/sso-azure-step-06-1.png)

All of the correct information should automatically populate the _Basic SAML Configuration_ screen as shown.

![Azure AD console: Supabase application, SAML-based Sign-on screen, Basic SAML Configuration shown](https://supabase.com/docs/img/sso-azure-step-06-2.png)

**Make sure you input these additional settings.**

| Setting | Value |
| --- | --- |
| Sign on URL | `https://supabase.com/dashboard/sign-in-sso` |
| Relay State | `https://supabase.com/dashboard` |

Finally, click the _Save_ button to save the configuration.

## Step 7: Obtain metadata URL and send to Supabase [\#](https://supabase.com/docs/guides/platform/sso/azure\#send-metadata-url)

Supabase needs to finalize enabling single sign-on with your Azure AD application. To do this, copy and send the link under **App Federation Metadata URL** in \*section 3 **SAML Certificates\*** to your support contact and await further instructions. If you're not clear who to send this link to or need further assistance, reach out to [Supabase Support](https://supabase.help/).

**Do not test the login until you have heard back from the support contact.**

![Azure AD console: Supabase application, SAML Certificates card shown, App Federation Metadata Url highlighted](https://supabase.com/docs/img/sso-azure-step-07.png)

## Step 8: Wait for confirmation [\#](https://supabase.com/docs/guides/platform/sso/azure\#confirmation)

Wait for confirmation or further instructions from your support contact at Supabase before proceeding to the next step. It usually takes us 1 business day to configure SSO for you.

## Step 9: Test single sign-on [\#](https://supabase.com/docs/guides/platform/sso/azure\#testing)

_Testing sign-on before your Azure AD has been registered with Supabase will not work. Make sure you've received confirmation from your support contact at Supabase as laid out in the [confirmation](https://supabase.com/docs/guides/platform/sso/azure#confirmation) step._

Once you’ve received confirmation from your support contact at Supabase that SSO setup has been completed for your enterprise, you can ask some of your users to sign in via their Azure AD account.

You ask them to enter their email address on the [Sign in with SSO](https://supabase.com/dashboard/sign-in-sso) page.

If sign in is not working correctly, reach out to your support contact at Supabase for further guidance.

### Is this helpful?

NoYes

### On this page

[Step 1: Add and register an Enterprise application](https://supabase.com/docs/guides/platform/sso/azure#add-and-register-enterprise-application) [Step 2: Choose to create your own application](https://supabase.com/docs/guides/platform/sso/azure#create-application) [Step 3: Fill in application details](https://supabase.com/docs/guides/platform/sso/azure#add-application-details) [Step 4: Set up single sign-on](https://supabase.com/docs/guides/platform/sso/azure#set-up-single-sign-on) [Step 5: Select SAML single sign-on method](https://supabase.com/docs/guides/platform/sso/azure#saml-sso) [Step 6: Upload SAML-based sign-on metadata file](https://supabase.com/docs/guides/platform/sso/azure#upload-saml-metadata) [Step 7: Obtain metadata URL and send to Supabase](https://supabase.com/docs/guides/platform/sso/azure#send-metadata-url) [Step 8: Wait for confirmation](https://supabase.com/docs/guides/platform/sso/azure#confirmation) [Step 9: Test single sign-on](https://supabase.com/docs/guides/platform/sso/azure#testing)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings