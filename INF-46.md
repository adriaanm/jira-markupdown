That one is for Paul:

Here's a patch to trac 0.11.6 (seems to apply to trunk as well) which by default doesn't send any notifications if only cc has changed. It can be disabled by setting

{code}
[notification]
no_cc_notification = false
{code}
