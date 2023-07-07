---
title: Regulatory and Industry Compliance in CockroachDB Cloud
summary: Learn about the regulatory and industry compliance standards met by CockroachDB Cloud.
toc: true
docs_area: manage.security
---

When configured appropriately, {{ site.data.products.db }} meets the requirements of the following regulatory and industry compliance standards:

- **System and Organization Controls (SOC) 2 Type 2**: {{ site.data.products.db }} meets the requirements of SOC 2 Type 2, which is established and administered by the American Institute of Certified Public Accountants (AICPA). This certification means that the design and implementation of the controls and procedures that protect CockroachDB Cloud meet the relevant trust objectives both at a point in time and over a period of time.

    To learn more, refer to [SOC 2 Type 2 certification](https://www.cockroachlabs.com/blog/soc-2-compliance-2/) in our blog or contact your Cockroach Labs account representative.

- **Payment Card Industry Data Security Standard (PCI DSS)**: {{ site.data.products.dedicated }} has been certified by a PCI Qualified Security Assessor (QSA) as a PCI DSS Level 1 Service Provider. When configured appropriately, {{ site.data.products.dedicated }} advanced clusters meet the requirements of PCI DSS 3.2.1. PCI DSS is mandated by credit card issuers but administered by the [Payment Card Industry Security Standards Council](https://www.pcisecuritystandards.org/). Many organizations that do not store cardholder data still rely on compliance with PCI DSS to help protect other sensitive or confidential data or metadata.

    To learn more, refer to [PCI DSS Compliance in {{ site.data.products.dedicated }} advanced](pci-dss.html).

- **Health Insurance Portability and Accountability Act (HIPAA)**: The Health Insurance Portability and Accountability Act of 1996, commonly referred to as _HIPAA_, defines standards for the storage and handling of protected health information (PHI) data, which is personally-identifiable information (PII) related to patient healthcare and health insurance. When configured appropriately for [PCI DSS](pci-dss.html), {{ site.data.products.dedicated }} advanced also meets the requirements of HIPAA.
