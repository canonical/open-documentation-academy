# What is a glossary?

A *glossary* gives the meanings of *terms* in a work or domain.
It is presented as a list of terms in alphabetic order, each with a *gloss* that explains the term.

Each explanation gives the reader enough understanding to become familiar, and perhaps even comfortable, with the term.

Simplicity is prized. A glossary entry should aim to make the reader one step more comfortable with the term.

## Structure

### Definition

A definition is a statement of fact that conveys the meaning of a term. 
A definition that consists of a simple phrase or sentence may be all the explanation that is needed. For example:

```{glossary}
core
 The working memory of a computer.
```

### Abbreviation

If the term is an abbreviation, the gloss starts with the expanded form of the term. The abbreviation is explained briefly after this expansion. For example:

```{glossary}
DNS
 **Domain Name System**. A system that translates human-readable domain names (canonical.com) to their IP addresses (185.125.190.20).
```

In a conversational style, the definition has more of the structure and tone of a spoken sentence.
For example, the definition may begin by repeating the term being defined.

```{glossary}
FC
 **Fiber Channel** is a storage networking protocol used for low-latency communication between a storage device and a node
 in a Storage Area Network (SAN).
```

In this definition of FC, the abbreviation SAN is preceded by its expansion as is customary in running text.
This is a good way to handle abbreviations that may be unfamiliar or have not yet been formally defined.

### Cross-references

Cross-references help readers navigate among related terms.

If the gloss for a term discusses a second term, the second term should be linked.

<!-- {term}`DNS` replaced by *DNS* -->
<!-- other terms: LDAP, SSL, TLS, DTLS -->


```{glossary}
DDNS
 **Domain Name System**.
 A service that automatically updates {term}`DNS` records when the underlying IP address changes (aka, dynamic IP).
```

When a concept that provides depth or broader context is used in a gloss, it can be linked using "See".

```{glossary}
DIT
 **Directory Information Tree**. In directory services (See {term}`LDAP`), a hierarchical tree-like structure used to organize and store information.
```

A collection of related topics can be provided at the end of the gloss. These could link elsewhere in the documentation, or to external resources.

```{glossary}
GnuTLS
 **GNU’s Not Unix Transport Layer Security**.
 A GNU software package that secures data in transit by implementing the {term}`SSL`, {term}`TLS`, and {term}`DTLS` protocols.

 Related topic(s): [GnuTLS (Ubuntu Server documentation)](https://documentation.ubuntu.com/server/explanation/crypto/gnutls/#), [GnuTLS (official site)](https://www.gnutls.org/), Cryptography, Web services, OpenLDAP.

```

## Advanced features

In this treatment, the features considered basic are those focused on meaning.

There is a further realm of more specialized glossary features. Sometimes the origin of a term, or the pronunciation, or an alternate form, or some other aspect of the term is of great interest.

Advanced features can be introduced in glossaries by imitating the features of entries in authoritative dictionaries.

## Appendix 1: Mechanics

This document is written in Markedly Structured Text ([MyST](https://myst-parser.readthedocs.io/en/latest/syntax/typography.html#syntax-glossaries)).

Glossaries can also be produced using [reStructured Text (rST)](https://sublime-and-sphinx-guide.readthedocs.io/en/latest/glossary.html).

A reference to a term is coded as follows:

`{term}`referenced_term``

A complex glossary entry is coded as follows:

  ```{glossary}
  GnuTLS_example
   **GNU’s Not Unix Transport Layer Security**.
   A GNU software package that secures data in transit by implementing the :term:`SSL`, :term:`TLS`, and :term:`DTLS` protocols.

   Related topic(s): [GnuTLS (Ubuntu Server documentation)](https://documentation.ubuntu.com/server/explanation/crypto/gnutls/#), [GnuTLS (official site)](https://www.gnutls.org/), Cryptography, Web services, OpenLDAP.

  ```

## Appendix 2: Locally-defined terms

These terms are referenced within the topic to illustrate cross-references.

```{glossary}
datagram
 In networking, a self-contained, independent packet sent over a network.
 A datagram can be routed from source to destination without relying on earlier or subsequent transfers.

DTLS
 **Datagram Transport Layer Security**. A protocol that provides security for :term:`datagram`-based communication, such as UDP.
 DTLS offers security features similar to TLS, but is adapted because datagram protocols are connectionless.

LDAP
 **Lightweight Directory Access Protocol**.

SSL
 **Secure Socket Layer**.

TLS
 **Transport Layer Security**.

```
