# Argocd

## argo projects

if you use argo project != default you might need to add additional configuration to them e.g. "Source Repositories" or "Destinations" in order that the argo applications in the project work as expected. To make it work you can also use "*" for these configurations, but if possible better be specific. 

## Self-signed & Untrusted TLS Certificates

If a git repository uses a self signed certificate you might see the exception "failed to verify certificate x509 certificate signed by unknown authority" when creating a argo app.

You can add the certficate via settings, for details [see official doc](https://argo-cd.readthedocs.io/en/stable/user-guide/private-repositories/#managing-tls-certificates-using-the-argocd-web-ui). 

It seems that the easiest way is to add the root ca. It seems that if the CN is not correct for the "Repository Server Name" the validation fails in argo (but to be honest I am not entirely sure). Double check that you have written the exact "Repository Server Name" that was shown in the exception. It also seems that.
