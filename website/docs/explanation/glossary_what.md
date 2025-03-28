# What is a glossary?

## Purpose

This topic will provide background information on glossaries:
* What they are for
* How they are structured
* What advanced features are available

## What is a glossary?

A *glossary* gives the meanings of *terms* in a work or domain.
It is presented as a list of terms in alphabetic order, each with a *gloss* that explains the term.

Each explanation gives the reader enough understanding to become familiar, and perhaps even comfortable, with the term.

Simplicity is prized. As a rule of thumb, a glossary entry should make the reader one step more comfortable with the term.

## Structural features

### Simple definition

A definition is a statement of fact that conveys the meaning of a term. 
A definition that consists of a simple phrase or sentence may be all the explanation that is needed.

**core** <br />
The working memory of a computer.

When a formal style is used, the item being defined is omitted from the beginning of the explanation.

### Abbreviation

If the term is an abbreviation, the gloss starts with its expansion. The abbreviation is explained briefly after its expansion.

**DNS** <br />
**Domain Name System**. A system that translates human-readable domain names (canonical.com) to their IP addresses (185.125.190.20).

### Abbreviation (conversational)

A more conversational style is sometimes used, especially when the expansion of an abbreviation can be read as the subject of the gloss.

**FC** <br />
Fiber Channel (FC) is a storage networking protocol used for low-latency communication between a storage device and a node
in a Storage Area Network (SAN).

In this definition of FC, expansions are followed by abbreviations as is customary in running text.
This is a good way to handle abbreviations such as FC and SAN that are comparatively specialized.

### Cross-references

Cross-references help readers navigate among related terms.

#### A casual mention of another term

If the gloss for a term discusses a second term, the second term should be linked.

<!-- {term}`DNS` replaced by *DNS* -->
<!-- other terms: LDAP, SSL, TLS, DTLS -->


**DDNS** <br />
**Dynamic Domain Name System**. A service that automatically updates (term-ref) *DNS* records when the underlying IP address changes (aka, dynamic IP).

#### A term that provides depth or broader context

When a concept that provides depth or broader context is used in a gloss, it can be linked using "See".

**DIT** <br />
**Directory Information Tree**. In directory services (See (term-ref) *LDAP*), a hierarchical tree-like structure used to organize and store information.

#### A related topic

A collection of related topics can be provided at the end of the gloss.

**GnuTLS** <br />
**GNU’s Not Unix Transport Layer Security**. A GNU software package that secures data in transit by implementing the (term-ref) *SSL*, (term-ref) *TLS*, and (term-ref) *DTLS* protocols.

Related topic(s): [GnuTLS (Ubuntu Server documentation)](https://documentation.ubuntu.com/server/explanation/crypto/gnutls/#), [GnuTLS (official site)](https://www.gnutls.org/), Cryptography, Web services, OpenLDAP.

## Advanced features

In this treatment, the features considered basic are those focused on meaning.

There is an additional realm of glossary features that are focused on terminology management. Sometimes the origin of a term, or the pronunciation, or an alternate form, or some other aspect of the term is of great interest.

Advanced features can be introduced in glossaries by imitating the features of entries in authoritative dictionaries.
