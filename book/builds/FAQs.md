# FAQs - Building from sources

## OpenSSL system path
> Why do I need to amend the system path variable to include OpenSLL?

For two reasons:
* To be able to build these two projects in Visual Studio: `graphene_egenesis_brief` and `graphene_egenesis_full`. Alternatively, you could add the OpenSSL path for those projects individually inside Visual Studio.

* To be able to run the witness node - it needs access to the OpenSSL binaries.