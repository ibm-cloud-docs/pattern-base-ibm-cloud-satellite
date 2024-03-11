---

copyright:
  years: 2024
lastupdated: "2024-03-11"

subcollection: pattern-base-ibm-cloud-satellite

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Storage design
{: #storage-design}

<!-- text for storage design considerations goes here -->

There are multiple storage options when it comes to configuring [storage in a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-reqs-host-storage). Local storage, OpenShift Data Foundation (ODF), Azure file, AWS EBS, NetApp Ontap are some of the available options. Additional storage considerations include:

- {{site.data.keyword.satelliteshort}} location hosts must have a boot device with an 'ext4' file system and enough space to boot the host and run the operating system.
- While a minimum of 10 GiB is required, 25 GiB is recommended.
- In addition, *tmp* directory and *usr* directory must each have at least 1.5 GiB available.
- Hosts can't have a device that is mounted on directory /*var*/*data*.
- The *boot* partition must be a minimum of 1 GiB.

For more information, see [Understanding {{site.data.keyword.satelliteshort}} storage](/docs/satellite?topic=satellite-storage-template-ov).

