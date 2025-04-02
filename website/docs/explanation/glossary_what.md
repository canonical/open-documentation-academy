# What is a glossary?

A *glossary* gives the meanings of *terms* in a work or domain.
It is presented as a list of terms in alphabetic order, each with a *gloss* that explains the term.

Each explanation gives the reader enough understanding to become familiar, and perhaps even comfortable, with the term.

Simplicity is prized. A glossary entry should aim to make the reader one step more comfortable with the term.

## Structure

### Definition

A definition is a statement of fact that conveys the meaning of a term. 
A definition that consists of a simple phrase or sentence may be all the explanation that is needed.

```{glossary}
core
 The working memory of a computer.
```

When a formal style is used, the item being defined is omitted from the beginning of the explanation.

### Abbreviation

If the term is an abbreviation, the gloss starts with its expansion. The abbreviation is explained briefly after its expansion.

```{glossary}
DNS
 **Domain Name System**. A system that translates human-readable domain names (canonical.com) to their IP addresses (185.125.190.20).
```

A more conversational style is sometimes used, especially when the expansion of an abbreviation can be read as the subject of the gloss.

```{glossary}
FC
 Fiber Channel (FC) is a storage networking protocol used for low-latency communication between a storage device and a node
 in a Storage Area Network (SAN).
```

In this definition of FC, expansions are followed by abbreviations as is customary in running text.
This is a good way to handle abbreviations such as FC and SAN that are comparatively specialized.

### Cross-references

Cross-references help readers navigate among related terms.

If the gloss for a term discusses a second term, the second term should be linked.

<!-- {term}`DNS` replaced by *DNS* -->
<!-- other terms: LDAP, SSL, TLS, DTLS -->


```{glossary}
DDNS
 **Domain Name System**.
 A service that automatically updates :term:`DNS` records when the underlying IP address changes (aka, dynamic IP).
```

When a concept that provides depth or broader context is used in a gloss, it can be linked using "See".

```{glossary}
DIT
 **Directory Information Tree**. In directory services (See :term:`LDAP`), a hierarchical tree-like structure used to organize and store information.
```

A collection of related topics can be provided at the end of the gloss.

```{glossary}
GnuTLS
 **GNU’s Not Unix Transport Layer Security**.
 A GNU software package that secures data in transit by implementing the :term:`SSL`, :term:`TLS`, and :term:`DTLS` protocols.
```

Related topic(s): [GnuTLS (Ubuntu Server documentation)](https://documentation.ubuntu.com/server/explanation/crypto/gnutls/#), [GnuTLS (official site)](https://www.gnutls.org/), Cryptography, Web services, OpenLDAP.

## Advanced features

In this treatment, the features considered basic are those focused on meaning.

There is a further realm of glossary features that are focused on terminology management. Sometimes the origin of a term, or the pronunciation, or an alternate form, or some other aspect of the term is of great interest.

Advanced features can be introduced in glossaries by imitating the features of entries in authoritative dictionaries.
