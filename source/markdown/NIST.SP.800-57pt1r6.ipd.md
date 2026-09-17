                NIST Special Publication 800
                    NIST SP 800-57pt1r6 ipd

Recommendation for Key Management
                                       Part 1 — General

                                         Initial Public Draft

                                                 Elaine Barker
                                                William Barker



                This publication is available free of charge from:
               https://doi.org/10.6028/NIST.SP.800-57pt1r6.ipd


---

                                               NIST Special Publication 800
                                                   NIST SP 800-57pt1r6 ipd

Recommendation for Key Management
                                                                            Part 1 — General

                                                                               Initial Public Draft

                                                                                          Elaine Barker
                                                                        Computer Security Division
                                                                Information Technology Laboratory

                                                                                       William Barker
                                                               Information Technology Laboratory;
                                                                                    Strativia LLC*

                                                      *Former Strativia employee; some work for
                                                      this publication was done while at Strativia


                                               This publication is available free of charge from:
                                              https://doi.org/10.6028/NIST.SP.800-57pt1r6.ipd

                                                                                         December 2025




                                                                                U.S. Department of Commerce
                                                                                     Howard Lutnick, Secretary

                                                                National Institute of Standards and Technology
    Craig Burkhardt, Acting Under Secretary of Commerce for Standards and Technology and Acting NIST Director


---

NIST SP 800-57pt1r6 ipd (Initial Public Draft)                               Recommendation for Key Management
December 2025                                                                                   Part 1 — General

Certain equipment, instruments, software, or materials, commercial or non-commercial, are identified in this paper
in order to specify the experimental procedure adequately. Such identification does not imply recommendation or
endorsement of any product or service by NIST, nor does it imply that the materials or equipment identified are
necessarily the best available for the purpose.

There may be references in this publication to other publications currently under development by NIST in accordance
with its assigned statutory responsibilities. The information in this publication, including concepts and
methodologies, may be used by federal agencies even before the completion of such companion publications. Thus,
until each publication is completed, current requirements, guidelines, and procedures, where they exist, remain
operative. For planning and transition purposes, federal agencies may wish to closely follow the development of
these new publications by NIST.

Organizations are encouraged to review all draft publications during public comment periods and provide feedback
to NIST. Many NIST cybersecurity publications, other than the ones noted above, are available at
https://csrc.nist.gov/publications.


Authority
This publication has been developed by NIST in accordance with its statutory responsibilities under the Federal
Information Security Modernization Act (FISMA) of 2014, 44 U.S.C. § 3551 et seq., Public Law (P.L.) 113-283. NIST is
responsible for developing information security standards and guidelines, including minimum requirements for
federal information systems, but such standards and guidelines shall not apply to national security systems without
the express approval of appropriate federal officials exercising policy authority over such systems. This guideline is
consistent with the requirements of the Office of Management and Budget (OMB) Circular A-130.

Nothing in this publication should be taken to contradict the standards and guidelines made mandatory and binding
on federal agencies by the Secretary of Commerce under statutory authority. Nor should these guidelines be
interpreted as altering or superseding the existing authorities of the Secretary of Commerce, Director of the OMB,
or any other federal official. This publication may be used by nongovernmental organizations on a voluntary basis
and is not subject to copyright in the United States. Attribution would, however, be appreciated by NIST.


NIST Technical Series Policies
Copyright, Use, and Licensing Statements
NIST Technical Series Publication Identifier Syntax


Publication History
Approved by the NIST Editorial Review Board on YYYY-MM-DD [Will be added to final publication.]
Supersedes NIST Series XXX (Month Year) DOI [Will be added to final publication, if applicable.]


How to Cite this NIST Technical Series Publication
Barker E, Barker W (2025) Recommendation for Key Management: Part 1 — General. (National Institute of Standards
and Technology, Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-57pt1r6 ipd.
https://doi.org/10.6028/NIST.SP.800-57pt1r6.ipd


Author ORCID iDs
Elaine Barker: 0000-0003-0454-0461
William Barker: 0000-0002-4113-8861


Public Comment Period
December 5, 2025 – February 5, 2026


---

NIST SP 800-57pt1r6 ipd (Initial Public Draft)                          Recommendation for Key Management
December 2025                                                                              Part 1 — General

Submit Comments
keymanagement@nist.gov

National Institute of Standards and Technology
Attn: Computer Security Division, Information Technology Laboratory
100 Bureau Drive (Mail Stop 8930) Gaithersburg, MD 20899-8930


Additional Information
Additional information about this publication is available at https://csrc.nist.gov/pubs/sp/800/57/pt1/r6/ipd,
including related content, potential updates, and document history.


All comments are subject to release under the Freedom of Information Act (FOIA).


---

     NIST SP 800-57pt1r6 ipd (Initial Public Draft)                  Recommendation for Key Management
     December 2025                                                                      Part 1 — General


 1   Abstract
 2   This recommendation provides cryptographic key-management guidelines in three parts. Part 1
 3   provides general guidelines and best practices for the management of cryptographic keying
 4   material, including definitions for the security services that may be provided when using
 5   cryptography and the algorithms and key types that may be employed, specifications for the
 6   protection that each type of key and other cryptographic information requires and methods for
 7   providing this protection, discussions about the functions involved in key management, and
 8   discussions about a variety of key-management issues to be addressed when using cryptography.
 9   Part 2 provides guidance on policy and security planning requirements for U.S. Government
10   agencies. Part 3 provides guidelines for using the cryptographic features of current systems.

11   Keywords
12   archive; authentication; authorization; availability; backup; compromise; confidentiality;
13   cryptographic key; cryptographic module; digital signature; eXtendable-Output Function; hash
14   function; hashing method; key agreement; key management; key recovery; keying material; key
15   transport; private key; public key; quantum-resistant; secret key; security category; security
16   strength; trust anchor.

17   Reports on Computer Systems Technology
18   The Information Technology Laboratory (ITL) at the National Institute of Standards and
19   Technology (NIST) promotes the U.S. economy and public welfare by providing technical
20   leadership for the Nation’s measurement and standards infrastructure. ITL develops tests, test
21   methods, reference data, proof of concept implementations, and technical analyses to advance
22   the development and productive use of information technology. ITL’s responsibilities include the
23   development of management, administrative, technical, and physical standards and guidelines
24   for the cost-effective security and privacy of other than national security-related information in
25   federal information systems. The Special Publication 800-series reports on ITL’s research,
26   guidelines, and outreach efforts in information system security, and its collaborative activities
27   with industry, government, and academic organizations.
28




                                                      i


---

     NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
     December 2025                                                                    Part 1 — General

29   Note to Reviewers
30       1. Ascon, as specified in SP 800-232, and the new quantum-resistant algorithms specified in
31          FIPS 203, 204, and 205 have been included.
32       2. The keys used for both key establishment and key storage are now discussed separately.
33       3. The security categories used in the PQC competition have been included, along with the
34          quantum-resistant algorithms.
35       4. The time frames for algorithm approval status have been removed and replaced with
36          references to SP 800-131A.
37       5. A section has been added to discuss keying material storage and mechanisms.
38       6. See Appendix F for a more complete list of changes.
39




                                                      ii


---

     NIST SP 800-57pt1r6 ipd (Initial Public Draft)                     Recommendation for Key Management
     December 2025                                                                         Part 1 — General

40   Call for Patent Claims
41   This public review includes a call for information on essential patent claims (claims whose use
42   would be required for compliance with the guidance or requirements in this Information
43   Technology Laboratory (ITL) draft publication). Such guidance and/or requirements may be
44   directly stated in this ITL Publication or by reference to another publication. This call also includes
45   disclosure, where known, of the existence of pending U.S. or foreign patent applications relating
46   to this ITL draft publication and of any relevant unexpired U.S. or foreign patents.
47   ITL may require from the patent holder, or a party authorized to make assurances on its behalf,
48   in written or electronic form, either:
49       a) assurance in the form of a general disclaimer to the effect that such party does not hold
50          and does not currently intend holding any essential patent claim(s); or
51       b) assurance that a license to such essential patent claim(s) will be made available to
52          applicants desiring to utilize the license for the purpose of complying with the guidance
53          or requirements in this ITL draft publication either:
54               i.    under reasonable terms and conditions that are demonstrably free of any unfair
55                     discrimination; or
56              ii.    without compensation and under reasonable terms and conditions that are
57                     demonstrably free of any unfair discrimination.
58   Such assurance shall indicate that the patent holder (or third party authorized to make
59   assurances on its behalf) will include in any documents transferring ownership of patents subject
60   to the assurance, provisions sufficient to ensure that the commitments in the assurance are
61   binding on the transferee, and that the transferee will similarly include appropriate provisions in
62   the event of future transfers with the goal of binding each successor-in-interest.
63   The assurance shall also indicate that it is intended to be binding on successors-in-interest
64   regardless of whether such provisions are included in the relevant transfer documents.
65   Such statements should be addressed to: keymanagement@nist.gov.
66




                                                       iii


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                                     Recommendation for Key Management
      December 2025                                                                                                         Part 1 — General

 67   Table of Contents
 68   Executive Summary............................................................................................................................1
 69   1. Introduction ...................................................................................................................................3
 70
 71
 72
 73
 74
 75   2. Security Services ............................................................................................................................8
 76
 77
 78
 79
 80
 81
 82
 83   3. Cryptographic Algorithms .............................................................................................................14
 84
 85
 86
 87
 88   4. General Key-Management Guidelines ...........................................................................................17
 89
 90        4.1.1. Cryptographic Keys ..................................................................................................................... 17
 91        4.1.2. Other Related Information ......................................................................................................... 21
 92
 93
 94        4.3.1. Factors Affecting Cryptoperiods ................................................................................................. 23
 95        4.3.2. Consequence Factors Affecting Cryptoperiods .......................................................................... 24
 96        4.3.3. Other Factors Affecting Cryptoperiods ...................................................................................... 25
 97        4.3.4. Asymmetric Key Usage Periods and Cryptoperiods ................................................................... 25
 98        4.3.5. Symmetric Key Usage Periods and Cryptoperiods ..................................................................... 26
 99        4.3.6. Cryptoperiod Recommendations for Specific Key Types ........................................................... 27
100
101        4.4.1. Assurance of Integrity (Integrity Protection).............................................................................. 40


                                                                                iv


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                                        Recommendation for Key Management
      December 2025                                                                                                            Part 1 — General

102        4.4.2. Assurance of Algorithm Parameter Validity ............................................................................... 40
103        4.4.3. Assurance of Public-Key Validity ................................................................................................ 41
104        4.4.4. Assurance of Private-Key Possession ......................................................................................... 41
105        4.4.5. Key Confirmation ........................................................................................................................ 41
106
107        4.5.1. Implications ................................................................................................................................ 42
108        4.5.2. Protective Measures................................................................................................................... 43
109
110        4.6.1. Cryptographic Algorithm Security Strengths .............................................................................. 45
111        4.6.2. Using Algorithm Suites and the Effective Security Strength ...................................................... 52
112        4.6.3. Projected Algorithm and Security Strength Approval Status ..................................................... 54
113        4.6.4. Transitioning to New Algorithms and Key Sizes in Systems ....................................................... 55
114        4.6.5. Decrease in Security Over Time ................................................................................................. 60
115   5. Protection Requirements for Key Information ...............................................................................63
116
117        5.1.1. Summary of Protection and Assurance Requirements for Cryptographic Keys......................... 64
118        5.1.2. Summary of Protection Requirements for Other Related Information ..................................... 71
119
120        5.2.1. Protection Mechanisms for Key Information in Transit ............................................................. 75
121        5.2.2. Protection Mechanisms for Key Information in Storage ............................................................ 78
122        5.2.3. Metadata for Keys ...................................................................................................................... 87
123   6. Key States and Transitions ............................................................................................................89
124
125
126
127
128
129   7. Key-Management Phases and Functions .......................................................................................97
130
131        7.1.1. Entity Registration Function ..................................................................................................... 100
132        7.1.2. System Initialization Function .................................................................................................. 100
133        7.1.3. Initialization Function ............................................................................................................... 101
134        7.1.4. Keying-Material Installation Function ...................................................................................... 101
135        7.1.5. Key-Establishment Function ..................................................................................................... 101
136        7.1.6. Key Registration Function......................................................................................................... 114



                                                                                  v


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                                       Recommendation for Key Management
      December 2025                                                                                                           Part 1 — General

137
138        7.2.1. Normal Operational Storage Function ..................................................................................... 115
139        7.2.2. Continuity of Operations Function ........................................................................................... 116
140        7.2.3. Key Change Function ................................................................................................................ 119
141        7.2.4. Key Derivation Methods ........................................................................................................... 120
142
143        7.3.1. Key Archive and Key Recovery Functions ................................................................................. 121
144        7.3.2. Entity De-Registration Function ............................................................................................... 125
145        7.3.3. Key De-Registration Function ................................................................................................... 125
146        7.3.4. Key Destruction Function ......................................................................................................... 126
147        7.3.5. Key Revocation Function .......................................................................................................... 126
148
149   8. Additional Considerations .......................................................................................................... 129
150
151
152        8.2.1. Key Inventories ......................................................................................................................... 130
153        8.2.2. Certificate Inventories .............................................................................................................. 130
154
155
156
157        8.5.1. Backed Up and Archived Key .................................................................................................... 134
158        8.5.2. Key Recovery ............................................................................................................................ 134
159        8.5.3. System Redundancy/Contingency Planning ............................................................................. 136
160        8.5.4. Compromise Recovery.............................................................................................................. 138
161   References..................................................................................................................................... 140
162   Appendix A. Cryptographic and Non-Cryptographic Integrity and Source Authentication Mechanisms
163   ...................................................................................................................................................... 147
164   Appendix B. Key Recovery .............................................................................................................. 150
165
166        B.1.1. Recovery From Stored Keying Material.................................................................................... 151
167        B.1.2. Recovery by Reconstruction of Keying Material ...................................................................... 151
168        B.1.3. Conditions Under Which Keying Material Needs to be Recoverable....................................... 151
169        B.1.4. Key-Recovery Systems.............................................................................................................. 152
170        B.1.5. Key-Recovery Policy ................................................................................................................. 153
171



                                                                                 vi


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                                        Recommendation for Key Management
      December 2025                                                                                                            Part 1 — General

172       B.2.1. Private Signature Key ............................................................................................................... 154
173       B.2.2. Public Signature-Verification Key ............................................................................................. 155
174
175       B.3.1. Symmetric Authentication Key................................................................................................. 155
176       B.3.2. Authentication (Asymmetric) Key Pair ..................................................................................... 156
177
178
179
180       B.6.1. Symmetric Key-Wrapping Key .................................................................................................. 157
181       B.6.2. Key-Transport Key Pair ............................................................................................................. 157
182       B.6.3. Symmetric Key-Agreement Key................................................................................................ 158
183       B.6.4. Static Key-Agreement Key Pair ................................................................................................. 158
184       B.6.5. Ephemeral Key-Agreement Key Pair ........................................................................................ 159
185       B.6.6. Static Encapsulation/Decapsulation Key Pair ........................................................................... 160
186       B.6.7. Ephemeral Encapsulation/Decapsulation Key Pair .................................................................. 160
187
188       B.7.1. Encryption/Decryption of Data in Transit ................................................................................ 161
189       B.7.2. B.7.2          Encryption/Decryption of Data at Rest ...................................................................... 161
190
191       B.8.1. Symmetric Key Wrapping/Unwrapping Key ............................................................................. 163
192       B.8.2. Asymmetric Key Encryption/Decryption Key Pair .................................................................... 163
193       B.8.3. Encapsulation/Decapsulation Key Pair..................................................................................... 164
194
195       B.9.1. Symmetric Authorization Key................................................................................................... 164
196       B.9.2. Authorization Key Pair .............................................................................................................. 164
197
198       B.10.1. Algorithm Parameters ............................................................................................................ 165
199       B.10.2. Initialization Vector (IV).......................................................................................................... 165
200       B.10.3. Shared Secret ......................................................................................................................... 165
201       B.10.4. Seed ........................................................................................................................................ 166
202       B.10.5. Other Public and Secret Information ..................................................................................... 166
203       B.10.6. Intermediate Results .............................................................................................................. 166
204       B.10.7. Key-Control Information/Metadata ....................................................................................... 166
205       B.10.8. Random Numbers .................................................................................................................. 166
206       B.10.9. Password ................................................................................................................................ 166



                                                                                 vii


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                                   Recommendation for Key Management
      December 2025                                                                                                       Part 1 — General

207        B.10.10. Audit Information ................................................................................................................. 167
208   Appendix C. Security Strength Categories for Post-Quantum Algorithms ......................................... 168
209   Appendix D. List of Abbreviations and Acronyms ............................................................................ 170
210   Appendix E. Glossary...................................................................................................................... 174
211   Appendix F. Change Log ................................................................................................................. 186


212   List of Tables
213   Table 1. Suggested cryptoperiods for key types.................................................................................39
214   Table 2: Security Strength Categories for Post-quantum Algorithms ..................................................46
215   Table 3: Security strengths of symmetric block cipher algorithms ......................................................47
216   Table 4: Security strengths of classical asymmetric-key algorithms ....................................................48
217   Table 5:Security strengths of quantum-resistant asymmetric-key algorithms.....................................49
218   Table 6: Maximum security strengths for hash methods and hash-based functions ............................51
219   Table 8. Protection requirements for cryptographic keys ..................................................................65
220   Table 9. Protection requirements for other related information ........................................................72
221   Table 10. Backup of keys ................................................................................................................ 117
222   Table 11. Backup of other related information ................................................................................ 118
223   Table 12. Archive of keys ................................................................................................................ 122
224   Table 13. Archive of other related information ............................................................................... 124


225   List of Figures
226   Figure 1. Symmetric-key cryptoperiod ..............................................................................................27
227   Figure 2. Algorithm originator-usage period example ........................................................................56
228   Figure 3. Key state and transition example .......................................................................................89
229   Figure 4. Key-management phases ...................................................................................................98
230   Figure 5. Key-management states and phases ...................................................................................99
231   Figure 6: Example of a tree of keys in storage ................................................................................. 162
232




                                                                              viii


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
      December 2025                                                                   Part 1 — General

233   Acknowledgments
234   The National Institute of Standards and Technology (NIST) gratefully acknowledges and
235   appreciates the contributions of previous authors on the many security issues associated with
236   this document: William Burr and Timothy Polk from NIST, Miles Smid from Orion Security, and
237   Lydia Zieglar from the National Security Agency. NIST also thanks the many contributions from
238   the public and private sectors whose thoughtful and constructive comments improved the quality
239   and usefulness of this publication.
240




                                                       ix


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
      December 2025                                                                   Part 1 — General


241   Executive Summary
242   Cryptography is used to secure communications over networks, to protect information stored
243   in databases, and for many other critical applications. Cryptographic keys play an important
244   part in the operation of cryptography. These keys are analogous to the combination of a safe.
245   If a safe combination is known to an adversary, the strongest safe provides no security against
246   penetration.
247   The proper management of cryptographic keys is essential to the effective use of
248   cryptography for security. Poor key management may easily compromise strong algorithms.
249   This recommendation provides guidelines for the management of a cryptographic key
250   throughout its life cycle, including its secure generation, storage, distribution, use, and
251   destruction.
252   Ultimately, the security of information protected by cryptography directly depends on the
253   strength of the keys, the effectiveness of cryptographic mechanisms and protocols associated
254   with the keys, and the protection provided to the keys. Secret and private keys need to be
255   protected against unauthorized disclosure, and all keys need to be protected against
256   modification.
257   Cryptographic keys are used across a broad range of systems and applications in enterprises,
258   many of which are managed by individuals who may not have expertise in key management.
259   Consequently, organizations must ensure that clear guidance and oversight is provided for
260   the proper management of keys, as well as controls to ensure that the guidance is being
261   followed and implemented.
262   Organizations and developers are presented with many choices in their use of cryptographic
263   mechanisms. Inappropriate choices may result in an illusion of security but with little or no
264   real security for the protocol or application. This recommendation provides background
265   information and establishes a framework to support appropriate decisions when selecting
266   and using cryptographic mechanisms.
267   Cryptographic modules are used to perform cryptographic operations using these keys. This
268   recommendation does not address the implementation details for cryptographic modules
269   that may be used to achieve the security requirements identified herein. These details are
270   addressed in Federal Information Processing Standards (FIPS) Publication 140 [FIPS 140-3]
271   and its associated implementation guidance and derived test requirements, which are
272   available at https://csrc.nist.gov/projects/cmvp.
273   This recommendation is divided into three parts:
274       •    Part 1 — General contains basic key-management guidelines
275       •    Part 2 — Best Practices for Key Management Organizations provides a framework and
276            general guidance to support establishing cryptographic key management.




                                                       1


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)          Recommendation for Key Management
      December 2025                                                              Part 1 — General

277       •    Part 3 — Application-Specific Key Management Guidance addresses the key-
278            management issues associated with currently available implementations that use
279            cryptography.
280




                                                       2


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                            Recommendation for Key Management
      December 2025                                                                                                Part 1 — General

281   1. Introduction
282   The use of cryptographic mechanisms is one of the strongest ways to provide security services
283   for communications, data storage, and other applications. The National Institute of Standards
284   and Technology (NIST) publishes FIPS and NIST Special Publications (SPs) that specify
285   cryptographic techniques for protecting controlled unclassified information (CUI). 1
286   Since NIST published the Data Encryption Standard (DES) in 1977, the suite of approved
287   standardized algorithms has grown. New classes of algorithms have been added, such as
288   secure hash functions, key-derivation functions, and asymmetric quantum-resistant
289   algorithms. The suite of algorithms provides different levels of cryptographic strength
290   through a variety of key lengths. The algorithms may be combined in many ways to support
291   increasingly complex protocols and applications. This NIST recommendation applies to
292   federal agencies that use cryptography to protect CUI. On a voluntary basis, this
293   recommendation may also be followed by other organizations that want to implement sound
294   security principles in their computer systems.
295   The proper management of cryptographic keys and other key information is essential to the
296   effective use of cryptography for security. Cryptographic keys are analogous to the
297   combination of a safe. If an adversary knows the combination, the strongest safe provides no
298   security against penetration. Similarly, poor key management may easily compromise strong
299   algorithms. Ultimately, the security of the information protected by cryptography directly
300   depends on the strength of the keys, the effectiveness of the mechanisms and protocols
301   associated with the keys, and the protection afforded to the keys. Cryptography can be
302   rendered ineffective by weak implementations, inappropriate algorithm pairing, poor
303   physical security, and weak (i.e., vulnerable) protocols.
304   Key management is the process of managing a key throughout its life cycle, including its
305   secure generation, storage, distribution, use, and destruction. Keys may be managed
306   manually, but an automated system is often required to oversee, automate, and secure the
307   key-management process. An automated system that performs key management is
308   commonly known as a (cryptographic) key-management system (see [SP 800-130] and [SP
309   800-152].)
310   NIST-approved cryptographic techniques are periodically reassessed for their continued
311   effectiveness. The algorithms specified in NIST standards (e.g., AES, SHA-2, and ECDSA) and
312   the cryptographic modules in which they reside have required conformance tests. Accredited
313   laboratories perform these tests on vendor implementations that claim conformance to the
314   standards. Vendors are required to modify nonconforming implementations so that they
315   meet all applicable requirements. Users of validated implementations can have a high degree
316   of confidence that validated implementations conform to the standards. If any technique is
317   found to be inadequate for the continued protection of government information, the NIST
318   standard is revised or discontinued. Since 1977, NIST has developed a cryptographic “toolkit”
319   of NIST standards2 that form a basis for the implementation of approved cryptography. Part

      1 CUI was previously referred to as “sensitive but unclassified information.”
      2 The toolkit consists of publications specifying algorithms and guidance for their use rather than software code.




                                                                            3


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                            Recommendation for Key Management
      December 2025                                                                                                Part 1 — General

320   1 references many of those standards and provides guidelines for how they may be properly
321   used to protect CUI. The process for developing NIST standards is documented in [NIST IR
322   7977].

323   1.1. Purpose
324   Organizations and developers are presented with many new choices in their use of
325   cryptographic mechanisms. Inappropriate choices may result in little or no real security for
326   the protocol or application. This recommendation provides background information and
327   establishes frameworks to support appropriate decisions when selecting and using
328   cryptographic mechanisms.

329   1.2. Audience
330   The audiences for this recommendation include system and application owners and
331   managers, cryptographic module developers, protocol developers, and system
332   administrators. The recommendation is provided in three parts, which have been tailored to
333   specific audiences:
334        1. Part 1 (i.e., this document) provides general key-management guidelines that are
335           intended to be useful to both system developers and system administrators. 3
336           Cryptographic module developers may benefit by acquiring a greater understanding
337           of the key-management features that are required to support specific applications.
338           Protocol developers may identify key-management characteristics associated with
339           specific suites of algorithms and acquire a greater understanding of the security
340           services provided by those algorithms. System administrators may use Part 1 and Part
341           3 to assist in determining configuration settings that would be most appropriate for
342           their systems.
343        2. Part 2 [SP 800-57p2] helps system and application owners (e.g., the information
344           security group within the organization) identify appropriate organizational key-
345           management infrastructures, establish organizational key-management policies, and
346           specify organizational key-management practices and plans.
347        3. Part 3 [SP 800-57p3] addresses the key-management issues associated with currently
348           available cryptographic mechanisms. It is intended to provide guidelines for system
349           installers, system administrators, and end users of existing key-management
350           infrastructures, protocols, and other applications as well as the people making
351           purchasing decisions for new systems using currently available technology.
352   Although some background information and rationale are provided for context and to
353   support the recommendations, this document assumes that the reader has a basic
354   understanding of cryptography. For background material, readers may refer to a variety of



      3 System administrators will require additional specific information when setting up their systems.




                                                                           4


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
      December 2025                                                                    Part 1 — General

355   NIST and commercial publications, including [SP 800-175B], which                       provides
356   recommendations for using cryptography and NIST’s cryptographic standards.

357   1.3. Scope
358   Part 1 of this recommendation discusses cryptographic algorithms, infrastructures, protocols,
359   implementations, applications, and the management thereof. All cryptographic algorithms
360   currently approved by NIST for the protection of controlled unclassified information are
361   within the scope of Part 1.
362   Part 1 focuses on issues involving the management of cryptographic keys: their generation,
363   use, and eventual destruction. Related topics (such as, algorithm selection and appropriate
364   key length, cryptographic policy, and cryptographic module selection) are also included.
365   This recommendation does not address the implementation details for cryptographic
366   modules that may be used to achieve the identified security requirements, which are
367   provided in [FIPS 140-3], CMVP implementation guidance [IGD_B], and derived test
368   requirements [SP 800-140]. Moreover, this recommendation does not address the
369   requirements or procedures for operating a key archive or backup capability other than
370   discussing the types of keying material that are appropriate to back up or include in an archive
371   and the protection to be provided to the keying material.
372   This recommendation often uses “requirement” terms, which have the following meaning in
373   this document:
374       1. Shall: This term is used to indicate a requirement of a FIPS or a requirement that must
375          be fulfilled to claim conformance to this recommendation. Shall may be combined
376          with not to become shall not.
377       2. Should: This term is used to indicate an important recommendation. Ignoring the
378          recommendation could result in undesirable results. Should may be combined with
379          not to become should not.

380   1.4. Preliminary Discussion of Terms
381   Part 1 of this recommendation uses several terms related to the management of
382   cryptographic key information. While each of these terms is defined in Appendix E, it may be
383   useful to compare these terms and show their relationships, since they will be used
384   throughout the document:
385       •    A cryptographic key is a parameter used in conjunction with a cryptographic algorithm
386            that determines its operation in such a way that an entity with knowledge of the key
387            can reproduce, reverse, or verify the operation, while an entity without knowledge of
388            the key cannot. Examples include a symmetric key used with AES to encrypt plaintext
389            data and decrypt ciphertext data, a private signature key used with a digital signature
390            algorithm to generate a digital signature, or a public signature-verification key used
391            with a digital signature algorithm to verify a digital signature.


                                                       5


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
      December 2025                                                                    Part 1 — General

392            Keys are owned and used by entities (e.g., individuals, organizations, devices, or
393            processes) that interact with other entities to conduct business. In the case of non-
394            human owners (i.e., an organization, device, or process), the owner is represented or
395            sponsored by one or more humans. For example, if the owner is an organization, then
396            several humans may be authorized to use the key, and the humans may be said to
397            represent the organization when conducting its business. A device or process may
398            own and use the key, but a human sponsor is responsible for managing the key (e.g.,
399            generating or replacing the key when required).
400       •    Keying material includes a cryptographic key and other material (e.g., an Initialization
401            Vector or algorithm parameters) to be used during the execution of a cryptographic
402            algorithm.
403       •    Metadata is the information associated with a key that describes its specific
404            characteristics, constraints, acceptable uses, ownership, and other details. Portions of
405            the metadata may be secret (e.g., the identity of the key’s owner, in some cases).
406       •    Key information is information about a particular key that includes all of the keying
407            material associated with that key and associated metadata related to that key.
408            Symmetric keys and the private keys of asymmetric-key (public-key) algorithms
409            require confidentiality protection, and some metadata elements may also require this
410            protection. All key information requires integrity protection.

411   1.5. Content and Organization
412   This document is organized as follows:
413       •    Section 1, Introduction, establishes the purpose, scope, and intended audience.
414       •    Section 2, Security Services, defines the security services that may be provided using
415            cryptographic mechanisms.
416       •    Section 3, Cryptographic Algorithms, provides background information regarding the
417            approved cryptographic algorithms that generate and use cryptographic keying
418            material.
419       •    Section 4, General Key-Management Guidelines, classifies the different types of keys
420            and other key information according to their uses, discusses cryptoperiods and
421            recommends appropriate cryptoperiods for each key type, provides
422            recommendations and requirements for other keying material, introduces the
423            concept of assurance of algorithm-parameter and public-key validity, discusses the
424            implications of a key compromise, and provides guidelines for cryptographic
425            algorithm and key-size selection, implementation, and replacement.
426       •    Section 5, Protection Requirements for Key Information, specifies the protection that
427            each type of key information requires and identifies methods for providing this
428            protection. These protection requirements should be of particular interest to
429            cryptographic module vendors and application implementers.


                                                       6


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                           Recommendation for Key Management
      December 2025                                                                               Part 1 — General

430        •    Section 6, Key State and Transitions, identifies the states in which a cryptographic key
431             may exist during its lifetime.
432        •    Section 7, Key-Management Phases and Functions, identifies four phases and a
433             multitude of functions involved in key management. This section should be of
434             particular interest to cryptographic module vendors and developers of cryptographic
435             infrastructure services.
436        •    Section 8, Additional Considerations, discusses access control, identity authentication,
437             inventory management, accountability, audit, and survivability.
438        •    The References list the sources cited in this document.
439        •    Appendix A, Cryptographic and Non-Cryptographic Integrity and Source-
440             Authentication Mechanisms, provides supplemental information about integrity and
441             source-authentication services.
442        •    Appendix B, Key Recovery, provides additional information about recovering keys
443             from key backups and archives.
444        •    Appendix C, Security Strength Categories for Post-Quantum Algorithms, provides a
445             collection of broad categories that have been developed to address uncertainties in
446             estimating the security strengths of post-quantum cryptosystems.
447        •    Appendix D, List of Abbreviations and Acronyms, includes all of the abbreviations and
448             acronyms used in this document.
449        •    Appendix E, Glossary, provides definitions for the terms used in this document.4
450        •    Appendix F, Change Log, contains a history of the changes made since the previously
451             published version of this document (i.e., Revision 5).
452




      4 Terms and definitions may be written differently in other documents.




                                                                         7


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
      December 2025                                                                   Part 1 — General

453   2. Security Services
454   Cryptography may be used to provide or support several basic security services:
455   confidentiality, identity authentication, integrity authentication, source authentication,
456   authorization, and non-repudiation. These services may also be required to protect a key and
457   information related to that key. In addition, there are other cryptographic and non-
458   cryptographic mechanisms that are used to support these security services. In general, a
459   single cryptographic mechanism may provide more than one service (e.g., the use of digital
460   signatures can provide integrity authentication and source authentication) but not all
461   services.

462   2.1. Confidentiality
463   Confidentiality is the property whereby information is not disclosed to unauthorized parties;
464   secrecy and privacy are terms that are often used synonymously with confidentiality.
465   Confidentiality can be obtained using encryption to render the information unintelligible
466   except by an authorized party that uses an appropriate key to decrypt the encrypted
467   information. For encryption to provide confidentiality, the cryptographic algorithm used for
468   encryption and its mode of operation must be designed and implemented so that an
469   unauthorized party cannot determine the decryption key associated with the encryption or
470   derive the plaintext directly without using the key.

471   2.2. Data Integrity
472   Data integrity is a property whereby data has not been modified in an unauthorized manner
473   since it was created, transmitted, or stored. Modification includes the insertion, deletion, and
474   substitution of data. Cryptographic mechanisms, such as message authentication codes
475   (MACs) or digital signatures, can be used to detect (with a high probability) both accidental
476   modifications (e.g., modifications that sometimes occur during noisy transmissions or by
477   hardware memory failures) and deliberate modifications by an adversary. Non-cryptographic
478   mechanisms are also often used to detect accidental modifications but cannot be relied upon
479   to detect deliberate modifications. A more detailed treatment of this subject is provided in
480   Appendix A.
481   In this recommendation, the statement that a cryptographic algorithm “provides data
482   integrity” means that the algorithm can be used to detect unauthorized modifications.
483   Authenticating integrity is discussed in the next section.

484   2.3. Authentication
485   Three types of authentication services can be provided using cryptography:
486       1. An identity authentication service is used to provide assurance of the identity of an
487          entity interacting with a system.




                                                       8


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
      December 2025                                                                     Part 1 — General

488       2. An integrity authentication service is used to verify that data has not been modified
489          (i.e., this service provides integrity protection).
490       3. A source authentication service is used to verify the identity of the entity that created
491          and/or sent information.
492   Source authentication and identity authentication are very similar but have different
493   purposes. For example, source authentication is concerned with who originated a message,
494   whereas identity authentication is used to gain access to some service.
495   Several cryptographic mechanisms may be used to provide authentication services. Most
496   commonly, digital signatures or MACs are used to provide authentication; some key-
497   establishment techniques may also provide authentication.
498   When multiple individuals are permitted to share the same identity or source authentication
499   information (e.g., a password or cryptographic key), it is sometimes called role-based
500   authentication.

501   2.4. Authorization
502   Authorization is concerned with providing an official sanction or permission to perform a
503   function or activity (e.g., to access a document or access a room). Authorization is considered
504   to be a security service that is often supported by a cryptographic service. Normally,
505   authorization is granted only after the execution of a successful identity authentication
506   service. A non-cryptographic analog of the interaction between identity authentication and
507   authorization is the examination of an individual’s credentials to establish their identity. After
508   verifying the individual’s identity and authorization to access some resource (e.g., a locked
509   room), the individual is often provided with a key (e.g., an authorization key) or password
510   that will allow access to that resource.
511   Identity authentication can also be used to authorize a role (e.g., a system administrator or
512   audit role) rather than identify an individual. Once authenticated for a role, an entity is
513   authorized for all of the privileges associated with that role.

514   2.5. Non-Repudiation
515   In key management, non-repudiation is a term associated with digital signature keys and
516   digital certificates that bind the name of the certificate subject to a public key. When non-
517   repudiation is indicated for a digital signature key, it means that the signatures created by
518   that key not only support the usual integrity and source authentication services of digital
519   signatures but may also (depending on the context of the signature) indicate commitment by
520   the certificate subject in the same sense that a handwritten signature on a document may
521   indicate commitment to a contract.
522   Non-repudiation in a key-management context is not the same as non-repudiation in a legal
523   context. Cryptographic mechanisms can be used to provide evidence of the source of




                                                       9


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                              Recommendation for Key Management
      December 2025                                                                                  Part 1 — General

524   information, but other elements are involved in making a legal determination of non-
525   repudiation.

526   2.6. Support Services
527   The basic cryptographic security services discussed in the previous subsections often require
528   other supporting services. For example, cryptographic services often require the use of key-
529   establishment and random number generation services. Key establishment is the process by
530   which cryptographic keys are securely established among entities using manual transport
531   methods (e.g., using key loaders), automated methods (e.g., key-transport and/or key-
532   agreement protocols), or a combination of automated and manual methods. Random
533   numbers are needed during the generation of cryptographic keys, challenge values, and
534   nonces [SP 800-175B].
535   When keying material and other data are no longer needed, there is often a requirement to
536   destroy5 the keying material and other information. All copies of that information need to be
537   destroyed by all entities privy to the information, whether in operational, backup, or archive
538   storage; on key loaders; or printed on paper or other media. When developing or selecting a
539   key-management capability, the method for destroying keying material and other
540   information needs to be considered for each type of media used. [SP 800-88] provides
541   guidelines for the sanitization of sensitive information.

542   2.7. Combining Services
543   In many applications, a combination of security services (e.g., confidentiality, integrity
544   authentication, source authentication, and/or non-repudiation) is desired. Designers of
545   secure systems often begin by considering which security services are needed to protect the
546   information stored and processed by the system. After these services have been identified,
547   the designer then considers what mechanisms will best provide these services. Not all
548   mechanisms are cryptographic in nature. For example, physical security may be used to
549   protect the confidentiality of certain types of data (e.g., by placing the data in a safe), and
550   identification badges or biometric identification devices may be used for identity
551   authentication. However, cryptographic mechanisms that consist of algorithms, keys, and
552   other keying material often provide an additional, cost-effective means of protecting the
553   security of information. This is particularly true in applications where the information would
554   otherwise be exposed to unauthorized entities.
555   When properly implemented, some cryptographic algorithms provide multiple services. For
556   example:
557        •     A MAC can provide both source and integrity authentication if the symmetric keys are
558              unique to each pair of entities and known only by those entities (see [SP 800-175B]).



      5 Other terms often used are delete, destroy, zeroize, and sanitize.




                                                                             10


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                  Recommendation for Key Management
      December 2025                                                                      Part 1 — General

559       •    A digital signature algorithm can provide identity, integrity, and source authentication
560            as well as non-repudiation (see [SP 800-175B]).
561       •    Certain modes of operation can provide confidentiality, integrity authentication, and
562            source authentication when properly implemented. These modes should be
563            specifically designed to provide these services.
564   However, different algorithms and procedures typically need to be employed to provide all
565   of the desired services. For example, consider a system in which the secure exchange of
566   information between pairs of entities is needed. Some of the exchanged information requires
567   only integrity protection, while other information requires both integrity and confidentiality
568   protection. Each entity that participates in the information exchange must also know the
569   identity of the other entity. The designers of this example system decide that a public-key
570   infrastructure (PKI) needs to be established, and each individual who wants to communicate
571   securely is required to obtain the necessary public-key certificates after physically proving
572   their identity. A PKI includes one or more certification authorities (CAs) that are responsible
573   for creating certificates and usually at least one registration authority (RA) associated with
574   each CA. The RA is responsible for confirming the identities of entities requesting certificates.
575   The identity-proving process requires the presentation of proper credentials, such as a
576   driver’s license, passport, or birth certificate.
577   Two types of public-key certificates are commonly used: certificates used for digital
578   signatures and certificates used for key establishment.
579       1. To obtain a digital signature certificate, an individual generates a pair of keys for a
580          specific digital signature algorithm (e.g., RSA, ECDSA, or ML-DSA); that individual is the
581          owner of the key pair. The key pair consists of a public key and a private key that
582          correspond to each other and can be used only with the specific algorithm. The public
583          key of the key pair is included in the certificate along with an identifier to be used by
584          the key-pair owner and other information. The certificate is digitally signed by a CA
585          using a digital signature private key owned by the CA, and the certificate is provided
586          to the key-pair owner, deposited in a repository, or both. The private key remains
587          under the sole control of the owner (i.e., the private key is kept secret).
588            When using digital signature certificates, one entity (i.e., a signatory) signs data using
589            the private key and sends the signed data to an intended recipient. The recipient:
590                o Obtains the signatory’s public-key certificate (e.g., from the recipient or a
591                  repository),
592                o Verifies the certificate using the CA’s public key that corresponds to the private
593                  key used to sign the certificate, and then
594                o Uses the public key in the certificate (i.e., the public key corresponding to the
595                  private key used by the signatory) to verify the signature on the received data.
596            By using this process, the recipient obtains assurances of both the integrity and the
597            source of the received data using a digital signature algorithm.



                                                       11


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                  Recommendation for Key Management
      December 2025                                                                      Part 1 — General

598       2. To obtain a certificate for key establishment, a key pair needs to be generated for a
599          specific key-establishment algorithm (e.g., RSA, Diffie-Hellman, or ML-KEM). As in the
600          case of digital signatures, the public key is placed in a certificate signed by a CA, and
601          the private key is kept secret by the key-pair owner.
602            Three methods of key establishment employing certificates are used: key agreement,
603            key transport, and key encapsulation (see [SP 800-175B]).
604                o Key agreement requires that when two entities wish to communicate, they
605                  need to exchange information (e.g., their key-establishment certificates
606                  containing their public keys and possibly other information) that allows both
607                  entities to generate the same keys without actually transmitting the keys
608                  between them.
609                o Key transport requires that one entity (i.e., the sender) select one or more
610                  keys to be sent to the other entity (i.e., the intended receiver) and encrypt
611                  them using the intended receiver’s certified public key before sending the
612                  encrypted key (i.e., the ciphertext) to the receiver for decryption. The
613                  originally generated key is retained by the sender.
614                o Key encapsulation requires that an encapsulating entity (i.e., the sender)
615                  generate a shared secret key and ciphertext using the intended receiver’s
616                  certified public key and a newly generated random value. The ciphertext is
617                  sent to the intended receiver for decapsulation, and the shared secret key is
618                  retained by the encapsulating sender. The receiver uses the ciphertext and its
619                  private key that corresponds to the certified public key to recover the same
620                  shared secret key retained by the encapsulating sender.
621            The key-establishment certificates are checked by verifying the CA’s signature on the
622            certificate before using the agreed-upon, transported, or decapsulated keys. These
623            keys can be used with a symmetric algorithm for encryption or message
624            authentication to provide confidentiality or integrity protection for transmitted data.
625            The receiver of the data protected by the symmetric keys has assurance that the data
626            came from the other entity indicated by the public-key certificate (i.e., source
627            authentication for the symmetric keys has been obtained).
628   These examples show how cryptographic algorithms may be used to support multiple security
629   services. However, the security of such systems depends on many factors, including:
630       •    The strength of the individual’s credentials (e.g., a driver’s license, passport, or birth
631            certificate) and the identity-authentication process;
632       •    The strength of the cryptographic algorithms used;
633       •    The degree of trust placed in the RA and CA;
634       •    The strength of the key-establishment protocols; and
635       •    The care taken by the users when generating their keys and protecting them from
636            unauthorized use.


                                                       12


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)             Recommendation for Key Management
      December 2025                                                                 Part 1 — General

637   Therefore, designing a security system that provides the desired security services by making
638   use of cryptographic algorithms and sound key-management techniques also requires careful
639   consideration of all factors and risks. The design and implementation of such systems should
640   be performed by analysts who have the necessary skills and expertise to effectively consider
641   and address these factors and risks.
642




                                                       13


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                     Recommendation for Key Management
      December 2025                                                                         Part 1 — General

643   3. Cryptographic Algorithms
644   FIPS-approved and NIST-recommended cryptographic algorithms shall be used whenever
645   cryptographic services are required. These approved algorithms have undergone an intensive
646   security analysis prior to their approval and continue to be examined to ensure that they
647   provide adequate security. Most cryptographic algorithms require cryptographic keys and
648   other keying material. In some cases, an algorithm may be strengthened by increasing the
649   key size used. Part 1 advises the users of cryptographic mechanisms on the appropriate
650   choices of algorithms and key sizes.
651              Important note: Cryptanalytic algorithms (e.g., Shor’s algorithms running
652              on future quantum computers) are projected to defeat the security
653              provided by classical approved asymmetric algorithms (i.e., RSA, ECDSA
654              and ECDH). A transition to quantum-resistant algorithms is underway. See
655              https://csrc.nist.gov/projects/post-quantum-cryptography for the status
656              of this effort.
657   This section describes the approved cryptographic algorithms that provide security services,
658   such as confidentiality, identity authentication, integrity authentication, and source
659   authentication. These services may be fulfilled using several different algorithms, although a
660   single algorithm may often be used to provide multiple services (see [SP 800-175B]).
661   There are three basic classes of approved cryptographic algorithms: cryptographic hashing
662   methods (see Sec. 3.1), symmetric-key algorithms (see Sec. 3.2), and asymmetric-key
663   algorithms (see Sec. 3.3). Any keys required for using these algorithms must be generated
664   using random bit generators (RBGs) (see Sec. 3.4).

665   3.1. Cryptographic Hashing Methods
666   In cryptography, hashing is the process of using an input bit string of arbitrary length to
667   produce an output with a given length, which is referred to as the “hash value.” 6
668   Cryptographic hashing methods do not require keys for their basic operation. Two categories
669   of hashing methods have been approved: cryptographic hash functions and eXtendable-
670   Output Functions (XOFs).
671   A cryptographic hash function is a cryptographic primitive that produces a fixed-length output
672   that is a condensed representation of its input (e.g., a message or other data). The number
673   of output bits is determined by the design of the hash function. The approved hash functions
674   are defined in [FIPS 180], [FIPS 202], and [SP 800-232]. Additionally, [SP 800-175B] provides
675   a brief description of how a hash function works.
676   A XOF produces an output that can be extended to any desired length (e.g., the desired length
677   is identified when requesting XOF execution). Approved XOFs for Federal Government use
678   are specified in [FIPS 202] and [SP 800-232].



      6 An alternative term for “hash value” is “message digest.”




                                                                    14


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
      December 2025                                                                     Part 1 — General

679   With a well-designed hash function or XOF, it is not feasible to construct or find input that
680   will produce a given hash value (i.e., pre-image resistance), nor is it feasible to find two inputs
681   that produce the same hash value (i.e., collision resistance).
682   Many algorithms and schemes that provide a security service use a hash function or XOF as a
683   component of the algorithm (i.e., use the hashing method as a building block for the
684   algorithm or scheme) to:
685       •    Provide source and integrity authentication services using a MAC (see item 2 in Sec.
686            3.2),
687       •    Derive keys from pre-shared keys (see item 3 in Sec. 3.2),
688       •    Compress messages for digital signature generation and verification (see item 1 in Sec.
689            3.3),
690       •    Derive keys using asymmetric key-establishment algorithms (see item 2 in Sec. 3.3),
691            or
692       •    Generate random numbers (see Sec. 3.4).

693   3.2. Symmetric-Key Algorithms
694   Symmetric-key algorithms (sometimes known as secret-key algorithms) transform data in a
695   way that is fundamentally difficult to undo without knowledge of the secret key. The key is
696   “symmetric” because the same key is used for a cryptographic operation and its inverse (e.g.,
697   for both encryption and decryption). Symmetric keys are often known by more than one
698   entity. However, the key shall be generated using a random process and shall not be
699   disclosed to entities that are not authorized to access the data protected by that algorithm
700   and key.
701   Three classes of symmetric-key algorithms have been approved: those based on block cipher
702   algorithms (e.g., AES, as specified in [FIPS 197]), those based on permutations (e.g., Ascon-
703   AEAD128, as specified in [SP 800-232]), and those based on the use of a hash function or XOF
704   (e.g., a keyed-hash MAC, as specified in [SP 800-224]). [SP 800-175B] provides discussions on
705   each algorithm type as well as the modes of operation that are used with block cipher
706   algorithms.
707   Symmetric-key algorithms may be used to:
708       •    Provide data confidentiality — The same key is used to encrypt and decrypt data (see
709            [FIPS 197], [SP 800-38A], [SP 800-38C], [SP 800-38D], and [SP 800-232]).
710       •    Provide source and integrity authentication services in the form of MACs — The same
711            key is used to generate the MAC and validate it. MACs normally employ either a
712            symmetric-key algorithm or a cryptographic hash function as their cryptographic
713            primitive (see CMAC, as specified in [SP 800-38B]; HMAC using a hash function, as
714            specified in [SP 800-224]; and KMAC, as specified in [SP 800-185]).




                                                       15


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                          Recommendation for Key Management
      December 2025                                                                                              Part 1 — General

715        •     Derive keying material from a pre-shared key using a key-derivation method (e.g., see
716              [SP 800-108])
717        •     Derive a key from a shared secret during the use of an asymmetric key-establishment
718              scheme (see [SP 800-56C])
719        •     Wrap keys using a key-wrapping algorithm (see [FIPS 197] and [SP 800-38F])
720        •     Generate random numbers (see Sec. 3.4)

721   3.3. Asymmetric-Key Algorithms
722   Asymmetric-key algorithms, commonly known as public-key algorithms, use two related keys
723   (i.e., a key pair) to perform their functions: a public key and a private key. The public key may
724   be known by anyone, while the private key should be under the sole control of the entity that
725   “owns” the key pair. 7 Even though the public and private keys of a key pair are related,
726   knowledge of the public key cannot be used to determine the private key. Asymmetric
727   algorithms may be used to:
728        •     Provide source, identity, and integrity authentication services in the form of digital
729              signatures or
730        •     Establish cryptographic keying material using key-establishment schemes.
731   Digital signatures are generated by the owner of a private key and can be verified by any party
732   that has the corresponding public key. Approved digital signature algorithms are specified in
733   [FIPS 186-5], [FIPS 204], [FIPS 205], and [SP 800-208].
734   Approved key-establishment schemes using asymmetric-key algorithms are specified in [SP
735   800-56A], [SP 800-56B], and [FIPS 203]. Additionally, [[SP 800-175B] discusses the use of
736   asymmetric-key algorithms to generate digital signatures and establish keying material.

737   3.4. Random Bit Generation
738   Random bit generators (RBGs) 8 are required for the generation of keying material, such as
739   keys and initialization vectors (IVs). RBGs generate sequences of random bits (e.g., 010011);
740   technically, RNGs translate those bits into numbers (e.g., 010011 is translated into the
741   number 19). However, the term “random number generator” (RNG) is commonly used to
742   refer to both concepts. The use of RBGs is discussed in [SP 800-175B], and approved RBGs
743   are specified in the SP 800-90 series of documents (i.e., [SP 800-90A], [SP 800-90B], and [SP
744   800-90C].




      7 Sometimes, a key pair is generated by a party that is trusted by the key owner and then provided to the key owner.
      8 RBGs may also be called “random number generators” or RNGs.




                                                                         16


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                  Recommendation for Key Management
      December 2025                                                                      Part 1 — General

745   4. General Key-Management Guidelines
746   This section classifies the different types of keys and other cryptographic information
747   according to their uses; discusses cryptoperiods and suggests appropriate cryptoperiods for
748   each key type; provides recommendations and requirements for other keying material;
749   introduces assurance of domain-parameter validity, public-key validity, and private-key
750   possession; discusses the implications of the compromise of keying material; and provides
751   guidelines for the selection, implementation, and replacement of cryptographic algorithms
752   and key sizes according to their security strengths.

753   4.1. Key Types and Other Information
754   There are several different types of cryptographic keys, each used for a different purpose.
755   However, some algorithms can be used for several different purposes, so the keys are listed
756   separately for each purpose below. In addition, there is other information that is specifically
757   related to cryptographic algorithms and keys. The generation of these keys is discussed in [SP
758   800-133].

759   4.1.1. Cryptographic Keys
760   Several different types of keys are defined below. The keys are identified according to their
761   classification as public, private, or symmetric (i.e., secret) keys, and their use is indicated. See
762   Table 8 in Section 5.1.1 for the required protections for each key type.
763       •    Digital signatures [FIPS 186-5][FIPS 204][FIPS 205][SP 800-208]:
764            1. Private signature key: A private signature key is the private-key component of an
765               asymmetric-key (public-key) pair that is used by a public-key algorithm to
766               generate digital signatures. When properly handled, a private signature key can
767               be used to provide source authentication, integrity authentication, and non-
768               repudiation of messages, documents, and stored data.
769            2. Public signature-verification key: A public signature-verification key is the public-
770               key component of an asymmetric-key (public-key) pair that is used by a public-key
771               algorithm to verify digital signatures that were signed with the corresponding
772               private signature key.
773       •    Authentication keys:
774            3. Symmetric authentication key: A symmetric authentication key is used with a
775               symmetric-key algorithm to provide identity authentication and integrity
776               authentication of communication sessions, messages, documents, and stored
777               data. For the authenticated-encryption modes of operation for symmetric-key
778               algorithms, a single key is used for both authentication and encryption modes. See
779               [SP 800-38B], [SP 800-38C], [SP 800-38D], [SP 800-185], [SP 800-224], and [SP 800-
780               232].



                                                       17


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                                 Recommendation for Key Management
      December 2025                                                                                                     Part 1 — General

781              4. Private authentication key: A private authentication key is the private-key
782                 component of an asymmetric-key (public-key) pair that is used with a public-key
783                 algorithm to provide assurance of the identity of an entity (i.e., identity
784                 authentication) when establishing an authenticated communication session or
785                 authorization to perform some action.9 See [FIPS 186-5], [FIPS 204], [FIPS 205],
786                 and [SP 800-208].
787              5. Public authentication key: A public authentication key is the public-key component
788                 of an asymmetric-key (public-key) pair that is used with a public-key algorithm to
789                 verify the identity of an entity (i.e., identity authentication) when establishing an
790                 authenticated communication session or authorization to perform some action.10
791        •     Keys for random bit/number generation:
792              6. Symmetric random number generation key: This key is used to generate random
793                 numbers or random bits. See the SP 800-90 series.
794        •     Key derivation:
795              7. Symmetric key-derivation key: A symmetric key-derivation key (sometimes called
796                 a master key) is used to derive other symmetric keys (e.g., data-encryption keys,
797                 key-wrapping keys) using symmetric cryptographic methods. See [SP 800-108] and
798                 [SP 800-135].
799        •     Key establishment (automated):
800              8. Symmetric key-wrapping key: A symmetric key-wrapping key (sometimes called a
801                 key-encrypting key) is used with a symmetric-key algorithm to encrypt (i.e., wrap)
802                 other keys. The key-wrapping key used to encrypt a key is also used to reverse the
803                 encryption operation (i.e., decrypt/unwrap the encrypted key). Depending on the
804                 algorithm with which the key is used, the key may also be used to provide integrity
805                 protection. See [SP 800-38F].
806              9. Public key-transport key: A public key-transport key is the public-key component
807                 of an asymmetric-key (public-key) pair that is used to encrypt keys using a public-
808                 key algorithm. Public key-transport keys are used to establish (i.e., securely
809                 distribute) symmetric keys (e.g., key-wrapping keys, data-encryption keys, MAC
810                 keys) and, optionally, other keying material (e.g., an IV). See [SP 800-56B].
811              10. Private key-transport key: A private key-transport key is the private-key
812                  component of an asymmetric-key (public-key) pair that is used to decrypt keys
813                  that have been encrypted with the corresponding public key-transport key using
814                  a public-key algorithm. See [SP 800-56B].
815              11. Symmetric key-agreement key: This symmetric key is used to establish symmetric
816                  keys (e.g., key-wrapping keys, data-encryption keys, MAC keys) and, optionally,


      9 While integrity protection is also provided, it is not the primary intention of this key.
      10 While integrity protection is also provided, it is not the primary intention of this key.




                                                                              18


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
      December 2025                                                                    Part 1 — General

817                other keying material (e.g., IVs) using a symmetric key-agreement algorithm.
818                There are currently no NIST-approved methods for symmetric key agreement.
819            12. Public static key-agreement key: A public static key-agreement key is the long-
820                term public-key component of an asymmetric-key (public-key) pair that is used to
821                establish symmetric keys (e.g., key-wrapping keys, data-encryption keys, MAC
822                keys) and, optionally, other keying material (e.g., IVs). See [SP 800-56A] and [SP
823                800-56B].
824            13. Private static key-agreement key: A private static key-agreement key is the long-
825                term private-key component of an asymmetric-key (public-key) pair that is used
826                to establish symmetric keys (e.g., key-wrapping keys, data-encryption keys, MAC
827                keys) and, optionally, other keying material (e.g., IVs). See [SP 800-56A] and [SP
828                800-56B].
829            14. Public ephemeral key-agreement key: A public ephemeral key-agreement key is
830                the public-key component of an asymmetric key pair that is used in a single key-
831                establishment transaction to establish one or more symmetric keys (e.g., key-
832                wrapping keys, data-encryption keys, MAC keys) and, optionally, other keying
833                material (e.g., IVs). See [SP 800-56A].
834            15. Private ephemeral key-agreement key: A private ephemeral key-agreement key is
835                the private-key component of an asymmetric-key (public-key) pair that is used in
836                a single key-establishment transaction to establish one or more symmetric keys
837                (e.g., key-wrapping keys, data-encryption keys, MAC keys) and, optionally, other
838                keying material (e.g., IVs). See [SP 800-56A].
839            16. Public static encapsulation key: A public static encapsulation key is the long-term
840                public-key component of an asymmetric-key (public-key) pair that is used during
841                the encapsulation process in multiple key-establishment transactions to
842                encapsulate symmetric keys (e.g., key-wrapping keys, data-encryption keys, MAC
843                keys). See [FIPS 203] and [SP 800-227].
844            17. Private static decapsulation key: A private static decapsulation key is the long-
845                term private-key component of an asymmetric-key (public-key) pair that is used
846                in multiple key-establishment transactions to decapsulate symmetric keys that
847                were encapsulated using the corresponding public static encapsulation key. See
848                [FIPS 203] and [SP 800-227].
849            18. Public ephemeral encapsulation key: A public ephemeral encapsulation key is the
850                public-key component of an asymmetric-key (public-key) pair that is used in a
851                single key-establishment transaction to encapsulate symmetric keys (e.g., a key-
852                wrapping key, data-encryption key, MAC keys). See [FIPS 203] and [SP 800-227].
853            19. Private ephemeral decapsulation key: A private ephemeral decapsulation key is
854                the private-key component of an asymmetric-key (public key) pair that is used in
855                a single key-establishment transaction to decapsulate symmetric keys that were




                                                       19


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                  Recommendation for Key Management
      December 2025                                                                      Part 1 — General

856                encapsulated using the corresponding public ephemeral encapsulation key. See
857                [FIPS 203] and [SP 800-227].
858       •    Key storage (for operational, backup, and archive storage):
859            20. Symmetric key-wrapping key: A symmetric key-wrapping key (sometimes called a
860                key-encrypting key) is used with a symmetric-key algorithm to wrap (i.e., encrypt
861                and protect the integrity of) other keys for storage (e.g., the key-wrapping key
862                used to wrap a key is also used to unwrap the wrapped key). Depending on the
863                algorithm with which the key is used, the key may also be used to provide integrity
864                protection. See [SP 800-38F].
865            21. Public key-wrapping key: A public key-wrapping key is the public-key component
866                of an asymmetric-key (public-key) pair that is used to wrap (i.e., encrypt and
867                protect the integrity of) keys using a public-key algorithm. This key is used to store
868                symmetric keys (e.g., key-wrapping keys, data-encryption keys, MAC keys) and,
869                optionally, other keying material (e.g., IVs). The RSA algorithm may be used for
870                this purpose, but there is currently no NIST publication that specifically discusses
871                this use.
872            22. Private key-unwrapping key: A private key-unwrapping key is the private-key
873                component of an asymmetric-key (public-key) pair that is used to unwrap (i.e.,
874                decrypt and verify the integrity of) a key that has been wrapped with the
875                corresponding public key-wrapping key using a public-key algorithm. There is
876                currently no NIST publication that specifically discusses this use.
877            23. Public encapsulation key: A public encapsulation key is typically the long-term
878                public-key component of an asymmetric-key (public-key) pair that is used during
879                the encapsulation process to encapsulate symmetric keys for storage (e.g., key-
880                wrapping keys, data-encryption keys, MAC keys). See [FIPS 203].
881            24. Private decapsulation key: A private decapsulation key is the private-key
882                component of an asymmetric-key (public-key) pair that is used to decapsulate
883                symmetric keys that were encapsulated using the corresponding public
884                encapsulation key. See [FIPS 203].
885       •    Authorization:
886            25. Symmetric authorization key: A symmetric authorization key is used to provide
887                privileges to an entity using a symmetric cryptographic method. The authorization
888                key is known by both the entity responsible for monitoring and granting access
889                privileges for authorized entities and the entity seeking access to resources. There
890                is currently no NIST publication that specifically discusses this use.
891            26. Private authorization key: A private authorization key is the private-key
892                component of an asymmetric-key (public-key) pair that is used to prove the
893                owner’s right to privileges (e.g., using a digital signature). See [FIPS 186-5], [FIPS
894                204], [FIPS 205], and [SP 800-208]. There is currently no NIST publication that
895                specifically discusses this use.


                                                       20


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                            Recommendation for Key Management
      December 2025                                                                                                Part 1 — General

896              27. Public authorization key: A public authorization key is the public-key component
897                  of an asymmetric-key (public-key) pair that is used to verify privileges for an entity
898                  that knows the associated private authorization key. See [FIPS 186-5], [FIPS 204],
899                  [FIPS 205], and [SP 800-208]. There is currently no NIST publication that
900                  specifically discusses this use.

901   4.1.2. Other Related Information
902   Other information used in conjunction with cryptographic algorithms and keys also needs to
903   be protected. See Table 6 in Section 5.1.2 for the required protections for each type of
904   information. Information to be protected may include:
905        •     Algorithm Parameters: Algorithm parameters are used in conjunction with some
906              algorithms to generate keys, create digital signatures, or establish keying material. For
907              elliptic curve algorithm, the algorithm parameters are called domain parameters. See
908              [FIPS 186-5], [SP 800-56A], [FIPS 203], [FIPS 204], [FIPS 205], [SP 800-208], and [SP
909              800-232].
910        •     Initialization Vectors: IVs are used by several modes of operation for encryption,
911              decryption, and the computation of MACs using block-cipher algorithms. See Sec. 4.2.
912        •     Shared Secrets: Shared secrets 11 are generated during a key-agreement process and
913              are not suitable for use as keys. See [SP 800-56A] and [SP 800-56B].
914        •     Seeds:
915                   o Seeds are used for the generation of deterministic random bits (e.g., used to
916                     generate keying material that must remain secret or private). See the SP 800-
917                     90 series.
918                   o Seeds are used during the generation of keying material according to an
919                     algorithm specification. See [FIPS 203], [FIPS 204], [FIPS 205], and [SP 800-
920                     208].
921        •     Other public information: Public information (e.g., a nonce) is often used in key-
922              establishment and key-confirmation processes.
923        •     Other secret information: Secret information may be included in the seeding of an
924              RBG or the establishment of keying material. See [SP 800-56A], [SP 800-56B], the SP
925              800-90 series, and [SP 800-108].
926        •     Intermediate results: The intermediate results in cryptographic operations often
927              needs protection.
928        •     Key-control information/metadata: Information related to the keying material (e.g., a
929              key identifier, the purpose intended for the key, or a counter) must be protected to
930              ensure that the associated keying material can be correctly used. The key-control


      11 These not the shared secret keys output by a key-encapsulation algorithm (e.g., see [FIPS 203]).




                                                                          21


---

      NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
      December 2025                                                                     Part 1 — General

931            information is included in the metadata associated with the keying material (see Sec.
932            5.2.3).
933       •    Random bits (or numbers): The random bits created by an RBG. See the SP 800-90
934            series.
935       •    Passwords: A password is used to acquire access to privileges and can be used as a
936            credential in a source-authentication or identity-authentication mechanism. A
937            password can also be used to derive cryptographic keys that are used to protect and
938            access data in storage. See [SP 800-132].
939       •    Audit information: Audit information contains a record of key-management events.

940   4.2. Key Usage
941   In general, a single key should be used for only one purpose (e.g., encryption, integrity
942   authentication, key wrapping, random bit generation, or digital signatures). There are several
943   reasons for this:
944       •    Using the same key for two different cryptographic processes may weaken the
945            security provided by one or both processes.
946       •    Limiting the use of a key limits the damage that could be done if the key is
947            compromised.
948       •    Some uses of keys interfere with each other. For example, consider an RSA key pair
949            used for both key transport and digital signatures. In this case, the private key is used
950            as both a private key-transport key to decrypt the encrypted keys and as a private
951            signature key to generate digital signatures. It may be necessary to retain the private
952            key used for key transport beyond the cryptoperiod of the corresponding public key
953            to decrypt the encrypted keys needed to access encrypted data. The private key used
954            for signature generation needs to be destroyed at the expiration of its cryptoperiod
955            to prevent its compromise (see Sec. 4.3.6). In this example, the longevity
956            requirements for the private key-transport key and the private digital signature key
957            contradict each other.
958   This principle does not preclude using a single key if the same process can provide multiple
959   services. This is the case, for example, when a digital signature provides integrity
960   authentication and source authentication using a single digital signature or when a single
961   symmetric key can be used to encrypt and authenticate data in a single cryptographic
962   operation (e.g., using an authenticated-encryption operation as opposed to separate
963   encryption and authentication operations) (see Sec. 2.7).
964   This recommendation permits the use of a private key-transport or key-agreement key to
965   generate a digital signature when requesting the (initial) certificate for a static key-
966   establishment key that was generated as specified in [FIPS 186-5] (see [SP 800-56A], [SP 800-
967   56B], and Sec. 7.1.5.1.1.2).




                                                       22


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                  Recommendation for Key Management
       December 2025                                                                      Part 1 — General

 968   4.3. Cryptoperiods
 969   A cryptoperiod is the time span during which a specific key is authorized for use by legitimate
 970   entities or the keys for a given system will remain in effect. A suitably defined cryptoperiod:
 971       •    Limits the amount of information that is available for cryptanalysis to reveal the key
 972            (e.g., the number of plaintext and ciphertext pairs encrypted with the key);
 973       •    Limits the amount of exposure if a single key is compromised;
 974       •    Limits the use of a particular algorithm (e.g., to its estimated effective lifetime);
 975       •    Limits the time available for attempts to penetrate physical, procedural, and logical
 976            access mechanisms that protect a key from unauthorized disclosure;
 977       •    Limits the period within which information may be compromised by the inadvertent
 978            disclosure of a cryptographic key to unauthorized entities; and
 979       •    Limits the time available for computationally intensive cryptanalysis.
 980   Cryptoperiods are sometimes defined by an arbitrary time period or maximum amount of
 981   data protected by the key. However, trade-offs associated with the determination of
 982   cryptoperiods involve the risk and consequences of exposure, which should be carefully
 983   considered when selecting the cryptoperiod (see Sec. 4.3.4). If a key is compromised, its
 984   cryptoperiod shall no longer be considered valid. See Sec. 4.5 for discussions on handling
 985   compromised keys.

 986   4.3.1. Factors Affecting Cryptoperiods
 987   Some factors that affect the length of a cryptoperiod include:
 988       •    The strength of the cryptographic mechanisms (e.g., the algorithm, key length, block
 989            size, or mode of operation)
 990       •    The embodiment of the mechanisms (e.g., a [FIPS 140-3] Level 4 implementation or a
 991            software implementation on a personal computer)
 992       •    The operating environment (e.g., a secure limited-access facility, open office
 993            environment, or publicly accessible terminal)
 994       •    Personnel turnover (e.g., of system administrators and CA system personnel)
 995       •    The volume of data flow or the number of transactions
 996       •    The security life of the data
 997       •    Limitations required for algorithm use (e.g., the maximum number of invocations to
 998            avoid nonce reuse)
 999       •    The security function (e.g., data encryption, digital signature, key derivation, or key
1000            protection)




                                                        23


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

1001       •    The re-keying method (e.g., keyboard entry, re-keying using a key-loading device
1002            where humans have no direct access to keys, or remote re-keying within a PKI)
1003       •    The re-keying or key-derivation process used
1004       •    The number of nodes in a network that share a common key
1005       •    The number of copies of a key and the distribution of those copies
1006       •    The threat to the information from adversaries (e.g., their perceived technical
1007            capabilities and financial resources to mount an attack)
1008       •    The threat to the information from new and disruptive technologies (e.g., quantum
1009            computers)
1010   In general, short cryptoperiods enhance security. For example, some cryptographic
1011   algorithms might be less vulnerable to cryptanalysis if the adversary has only a limited
1012   amount of information encrypted under a single key. However, when manual key-distribution
1013   methods are subject to human error and frailty, more frequent key changes might actually
1014   increase the risk of key exposure. In these cases, especially when very strong cryptography is
1015   employed in hardware, it may be more prudent to have fewer, well-controlled manual key
1016   distributions rather than more frequent, poorly controlled manual key distributions.
1017   When strong cryptography is employed, physical, procedural, and logical considerations for
1018   access protection often have more impact on cryptoperiod selection than algorithm and key-
1019   size factors. When approved algorithms, modes of operation, and key sizes are used,
1020   adversaries may be able to access keys by penetrating or subverting a system with less time
1021   and fewer resources than would be required to mount and execute a cryptographic attack.

1022   4.3.2. Consequence Factors Affecting Cryptoperiods
1023   The consequences of exposure are measured by the sensitivity of the information, the
1024   criticality of the processes protected by the cryptography, and the cost of recovering from
1025   the compromise of information or processes. Sensitivity refers to the lifespan of the
1026   information being protected (e.g., 10 minutes, 10 days or 10 years) and the potential
1027   consequences of a loss of protection for that information (e.g., the disclosure of the
1028   information to unauthorized entities). In general, as the sensitivity of the information or the
1029   criticality of the processes protected by cryptography increase, the length of the associated
1030   cryptoperiods should decrease to limit the damage that might result from each compromise.
1031   This is subject to the caveat regarding the security and integrity of the re-keying or key-
1032   derivation process (see Sec. 7.2.3 and 7.2.4). However, short cryptoperiods may be counter-
1033   productive, particularly if denial of service is a paramount concern and there is a significant
1034   potential for error in the re-keying or key-derivation process.




                                                        24


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                            Recommendation for Key Management
       December 2025                                                                                                Part 1 — General

1035   4.3.3. Other Factors Affecting Cryptoperiods

1036   4.3.3.1. Communications Versus Storage
1037   Keys that are used to protect the confidentiality of communication exchanges may often have
1038   shorter cryptoperiods than keys used for the protection of stored data. Cryptoperiods are
1039   generally made longer for stored data because the overhead of generating new keys and re-
1040   encrypting all data that was encrypted using the old keys may be burdensome.

1041   4.3.3.2. Cost of Key Revocation and Replacement
1042   In some cases, the costs associated with changing keys are painfully high. Examples include
1043   the decryption and subsequent re-encryption of very large or distributed databases and the
1044   revocation and replacement of a very large number of keys (e.g., where there are very large
1045   numbers of geographically and organizationally distributed key holders). In such cases, the
1046   expense of the security measures necessary to support longer cryptoperiods may be justified.
1047   In other cases, the cryptoperiod may be shorter than would otherwise be necessary (e.g., to
1048   limit the period of time during which a key-management system maintains status
1049   information).

1050   4.3.4. Asymmetric Key Usage Periods and Cryptoperiods
1051   For asymmetric-key key pairs, each key of the pair has its own cryptoperiod. One key of the
1052   key pair is used to apply cryptographic protection (e.g., create a digital signature). The other
1053   key of the key pair is used to process the protected information (e.g., verify a digital
1054   signature). The two cryptoperiods typically begin at the same time, but one of the
1055   cryptoperiods may need to be longer than the other. For example:
1056        •     In the case of digital signature key pairs, the private signature key is used to sign data
1057              (i.e., apply cryptographic protection), and the public signature-verification key is used
1058              to verify digital signatures (i.e., process information that has already been protected).
1059              For a private signature key that is used to generate digital signatures as a proof-of-
1060              origin (i.e., for source authentication), the cryptoperiod of the private key may be
1061              shorter than the cryptoperiod of the public signature-verification key. In this case, the
1062              private key is intended for use for a fixed period of time after which the key owner
1063              shall destroy 12 the private key. The public key may be available for a longer period to
1064              verify signatures.
1065              The cryptoperiod of a private source-authentication key that is used to sign challenge
1066              information is basically the same as the cryptoperiod of the corresponding public key


       12
         A simple deletion of the keying material might not completely obliterate the information. For example, erasing the information might
       require overwriting that information multiple times with other non-related information, such as random bits or all zero or one bits. Keys
       stored in memory for a long time can become “burned in.” Splitting the key into shares that are frequently updated can mitigate this problem
       [DiCrescenzo].



                                                                           25


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                  Recommendation for Key Management
       December 2025                                                                      Part 1 — General

1067            (i.e., the public source-authentication key). That is, when the private key will no longer
1068            be used to sign challenges, the public key is no longer needed.
1069       •    For key-transport keys, the public key-transport key is used to apply protection (i.e.,
1070            encrypt a key), and the private key-transport key is used to decrypt the encrypted key.
1071            The cryptoperiod during which the public key-transport key may be used for
1072            encryption may be shorter than the cryptoperiod of the corresponding private key-
1073            transport key used to decrypt the encrypted key.
1074       •    For a key-encapsulation mechanism (KEM), the public encapsulation key of the
1075            intended receiver is used by the sending entity to encapsulate a key generated by the
1076            sending entity before sending the resulting ciphertext to the intended receiver. The
1077            receiver’s corresponding private decapsulation key is subsequently used to
1078            decapsulate the received ciphertext. The cryptoperiod of the private decapsulation
1079            key may need to be longer than the cryptoperiod of the corresponding encapsulation
1080            key.
1081       •    For key-agreement algorithms, the cryptoperiods of the two keys of the key pair are
1082            usually the same, although there are exceptions. For example, for the KAS1 scheme
1083            in [SP 800-56B], the recipient’s public key is used to encrypt keying material sent to
1084            the recipient by the scheme’s originator. The recipient subsequently uses the
1085            corresponding private key to decrypt the received ciphertext keying material. The
1086            cryptoperiod of the recipient’s private key may need to be longer than the
1087            cryptoperiod of the corresponding public key.
1088   When public keys are distributed in public-key certificates, a certificate often includes a
1089   validity period for the public key indicated by the notBefore and notAfter dates in the
1090   certificate. Certificates may be renewed (e.g., a new certificate containing the same public
1091   key may be issued with a new validity period). The range of time covered by the original
1092   certificate and all renewed certificates for the same public key shall not extend beyond the
1093   cryptoperiod of the public key. See Sec. 4.3.6 for guidelines regarding specific key types.

1094   4.3.5. Symmetric Key Usage Periods and Cryptoperiods
1095   For symmetric keys, a single key is used for both applying the protection (e.g., encrypting or
1096   computing a MAC on data) and processing the protected information (e.g., decrypting the
1097   encrypted data or verifying a MAC). The period during which cryptographic protection may
1098   be applied to data is called the originator-usage period, and the period during which the
1099   protected information is processed is called the recipient-usage period. A symmetric key shall
1100   not be used to provide protection after the end of the originator-usage period. The recipient-
1101   usage period may extend beyond the originator-usage period (see Fig. 1). This permits
1102   information that has been protected by the originator to be processed by the recipient for an
1103   extended period after the protection has been applied. However, in many cases, the
1104   originator and recipient-usage periods are the same. The total “cryptoperiod” of a symmetric
1105   key is the period from the beginning of the originator-usage period to the end of the recipient-



                                                        26


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                         Recommendation for Key Management
       December 2025                                                                             Part 1 — General

1106   usage period (see Fig. 1), although the originator-usage period has historically been used as
1107   the cryptoperiod for the key.




1108
1109                                          Fig. 1. Symmetric-key cryptoperiod

1110   In some cases, predetermined cryptoperiods may not be adequate for the security life of the
1111   protected data. If the required security life exceeds the cryptoperiod, then the protection
1112   may need to be reapplied using a new key.
1113   Examples of the usage periods for symmetric keys include:
1114       •    When a symmetric key is only used for securing communications, the period from the
1115            originator’s application of protection to the recipient’s processing may be negligible.
1116            In this case, the key is authorized for either purpose during the entire cryptoperiod
1117            (i.e., the originator-usage period and the recipient-usage period are the same).
1118       •    When a symmetric key is used to protect stored information, the originator-usage
1119            period (i.e., when the originator applies cryptographic protection to stored
1120            information) may end much earlier than the recipient-usage period (i.e., when the
1121            stored information is processed). In this case, the cryptoperiod begins at the initial
1122            time authorized for the application of protection with the key and ends with the latest
1123            time authorized for processing using that key. In general, the recipient-usage period
1124            for stored information will continue beyond the originator-usage period so that the
1125            stored information may be authenticated or decrypted at a later time.
1126       •    When a symmetric key is used to protect stored information, the recipient-usage
1127            period may start after the beginning of the originator-usage period, as shown in Fig.
1128            1. For example, information may be encrypted before being stored on some storage
1129            media. At a later time, the key may be distributed in order to decrypt and recover the
1130            information.

1131   4.3.6. Cryptoperiod Recommendations for Specific Key Types
1132   The key type, usage environment, and data characteristics described above may affect the
1133   cryptoperiod required for a given key. This document suggests cryptoperiods for various key
1134   types as lengths of time. Other measures for the cryptoperiod (e.g., the number of uses of



                                                             27


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                            Recommendation for Key Management
       December 2025                                                                                                Part 1 — General

1135   each key for a given cryptographic algorithm to protect information) may require appropriate
1136   adjustments. The suggested cryptoperiods are based more on the effectiveness of access
1137   mechanisms for keys than on the strength of the cryptographic mechanisms that use the keys.
1138   For example, the assignment of a one-year cryptoperiod versus a two-year period is primarily
1139   driven by the concern that unauthorized physical or logical access to the key is more likely to
1140   occur within two years than within one year.
1141   The suggested cryptoperiods are only rough order-of-magnitude guidelines. Longer or
1142   shorter cryptoperiods may be warranted, depending on the consequences of compromised
1143   confidentiality or integrity compromises, the supported application, and the environment in
1144   which the keys will be used. When assigning a longer cryptoperiod, serious consideration
1145   should be given to the risks associated with doing so (see Sec. 4.3.1). Most of the suggested
1146   cryptoperiods are based on supporting maximum operational efficiency and assumptions
1147   regarding the minimum security criteria for the usage environment (see [FIPS 140-3] and [SP
1148   800-37]).
1149   The factors described in Sec. 4.3.1 through 4.3.3 should be used to determine actual
1150   cryptoperiods for specific usage environments. Given the use of an approved algorithm and
1151   an appropriate key size for the protected information, suggested cryptoperiods for each key
1152   type are as follows:
1153        •     Digital signatures
1154              1. Private signature key
1155                   Type Considerations: In general, the cryptoperiod of a private signature key may
1156                   be shorter than the cryptoperiod of the corresponding public signature-
1157                   verification key. When the corresponding public key has been certified by a CA,
1158                   the cryptoperiod for a private signature key ends no later than the notAfter date
1159                   on the last certificate issued for the public key. 13
1160                   Cryptoperiod: Given an expectation that the security of the key-storage and use
1161                   environment will increase as the sensitivity and/or criticality of the processes for
1162                   which the key provides integrity protection increases, a maximum cryptoperiod of
1163                   about one to three years is suggested. A private signature key shall be destroyed
1164                   at the end of its cryptoperiod.
1165              2. Public signature-verification key
1166                   Type Considerations: In general, the cryptoperiod of a public signature-verification
1167                   key may be longer than the cryptoperiod of the corresponding private signature
1168                   key. The cryptoperiod is, in effect, the period during which any signature
1169                   computed using the corresponding private signature key needs to be verified. A
1170                   longer cryptoperiod for a public signature-verification key (than that of the private
1171                   signature key) poses a relatively minimal security concern.



       13 Multiple consecutive certificates may be issued for the same public key, presumably with different notBefore and notAfter validity dates.




                                                                           28


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                             Recommendation for Key Management
       December 2025                                                                                 Part 1 — General

1172                    Cryptoperiod: The cryptoperiod may be a number of years longer than that of the
1173                    corresponding private signature key. However, due to the long exposure of
1174                    protection mechanisms to hostile attack, the reliability of the signature
1175                    verification is reduced with the passage of time. That is, for any given algorithm
1176                    and key size, vulnerability to cryptanalysis is expected to increase with time.
1177                    Although choosing the strongest algorithm available and a large key size can
1178                    minimize this vulnerability to cryptanalysis, the consequences of exposure to
1179                    attacks on physical, procedural, and logical access-control mechanisms for the
1180                    private key are not affected by algorithm selection.
1181                    Some systems use a cryptographic timestamping function to place an unforgeable
1182                    timestamp on each signed message. Even when the cryptoperiod of a private
1183                    signature key has expired, the corresponding public signature-verification key may
1184                    be used to verify signatures on messages whose timestamps are within the
1185                    cryptoperiod of the private signature key. In this case, one is relying on the
1186                    cryptographic timestamp function to provide assurance that the message was
1187                    signed within the cryptoperiod of the private signature key.
1188        •     Authentication keys
1189              3. Symmetric authentication key
1190                    Type Considerations: The cryptoperiod of a symmetric authentication key14
1191                    depends on the sensitivity of the type of information being protected and the
1192                    protection afforded by the key and associated algorithm. For very sensitive
1193                    information, an authentication key may need to be unique to the protected
1194                    information. For less sensitive information, a suitable cryptoperiod may extend
1195                    beyond a single use of the key. The originator-usage period of a symmetric
1196                    authentication key applies to the use of that key in applying the original
1197                    cryptographic protection for the information (e.g., computing the MAC). New
1198                    MACs shall not be computed on information using that key after the end of the
1199                    originator-usage period. However, the key may need to be available to verify the
1200                    MAC on the protected data beyond the originator-usage period (i.e., the recipient-
1201                    usage period may extend beyond the originator-usage period). The recipient-
1202                    usage period is the period during which a MAC that was generated during the
1203                    originator-usage period needs to be verified. However, if a MAC key is
1204                    compromised, it may be possible for an adversary to modify the data and then
1205                    recalculate the MAC.
1206                    Cryptoperiod: Given an expectation that the security of the key-storage and use
1207                    environment will increase as the sensitivity and/or criticality of the processes for
1208                    which the key provides integrity protection increases, an originator-usage period
1209                    of no more than two years is recommended. The recipient-usage period should
1210                    not extend more than three years beyond the end of the originator-usage period.


       14 This is used to enable data integrity and source authentication.




                                                                             29


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                                 Recommendation for Key Management
       December 2025                                                                                                     Part 1 — General

1211              4. Private authentication key
1212                    Type Considerations: A private authentication key may be used for multiple data
1213                    integrity processes and identity authentication events. In most cases, the
1214                    cryptoperiod of a private authentication key is the same as the cryptoperiod of
1215                    the corresponding public key.
1216                    Cryptoperiod: An appropriate cryptoperiod for a private authentication key would
1217                    be no more than one or two years, depending on its usage environment and the
1218                    sensitivity/criticality of the authenticated information.
1219              5. Public authentication key
1220                    Type Considerations: The cryptoperiod is, in effect, the period during which the
1221                    identity of the originator of the information protected by the corresponding
1222                    private authentication key needs to be verified (i.e., the identity needs to be
1223                    authenticated). 15
1224                    Cryptoperiod: In most cases, the cryptoperiod of a public authentication key is the
1225                    same as the cryptoperiod of the corresponding private authentication key.
1226        •     Keys for random bit/number generation
1227              6. Symmetric random number generation key
1228                    Type Considerations: A symmetric RBG key is used in a deterministic random bit
1229                    generation function. The approved RBGs in the SP 800-90 series control key
1230                    changes (e.g., during reseeding). The cryptoperiod consists of only an originator-
1231                    usage period.
1232                    Cryptoperiod: Assuming the use of an approved RBG, the maximum cryptoperiod
1233                    of a symmetric RBG key is determined by the design of the RBG (see the SP 800-
1234                    90 series).
1235        •     Key derivation
1236              7. Symmetric key-derivation key/master key
1237                    Type Considerations: A symmetric key-derivation key (also called a master key in
1238                    some environments) may be used multiple times to derive other keys using a (one-
1239                    way) key-derivation function or method (see Sec. 7.2.4). Therefore, the
1240                    cryptoperiod consists of only an originator-usage period for this key type. A
1241                    suitable cryptoperiod depends on the nature and use of the derived keys and on
1242                    considerations provided in Sec. 4.3. The cryptoperiod of a key derived from a key-
1243                    derivation key could be relatively short (e.g., used only for a single use,
1244                    communication session, or transaction). Alternatively, the key-derivation key
1245                    could be used over a longer period to derive (or re-derive) multiple keys for the
1246                    same or different purposes. The cryptoperiod of the derived keys depends on their
1247                    use (e.g., for symmetric data-encryption or integrity authentication).

       15 While integrity protection is also provided, it is not the primary intention of this key.




                                                                               30


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
       December 2025                                                                     Part 1 — General

1248                Cryptoperiod: An appropriate cryptoperiod for a symmetric key-derivation key
1249                might be one year, depending on its usage environment, the sensitivity/criticality
1250                of the information protected by the derived keys, and the number of keys derived
1251                from the key-derivation key.
1252       •    Key establishment (automated)
1253            8. Symmetric key-wrapping/unwrapping key
1254                Type Considerations: A symmetric key-wrapping key that is used to wrap (i.e.,
1255                encrypt and integrity protect) very large numbers of keys over a short period of
1256                time should have a relatively short originator-usage period. If a small number of
1257                keys are wrapped, the originator-usage period of the key-wrapping key could be
1258                longer. The originator-usage period of a symmetric key-wrapping key applies to
1259                the use of that key in providing key-wrapping protection for the keys. A wrapping
1260                operation shall not be performed using a key-wrapping key whose originator-
1261                usage period has expired. However, the key-wrapping key may need to be
1262                available to unwrap the protected keys (i.e., to decrypt and verify the integrity of
1263                the wrapped keys) beyond the originator-usage period (i.e., the recipient-usage
1264                period may need to extend beyond the originator-usage period). The recipient-
1265                usage period is the period during which keys that were wrapped during the key-
1266                wrapping key’s originator-usage period may need to be unwrapped.
1267                Some symmetric key-wrapping keys are used for only a single message or
1268                communication session. In the case of these very short-term key-wrapping keys,
1269                an appropriate cryptoperiod is a single communication session (i.e., includes both
1270                the originator and recipient-usage periods). If the wrapped keys are not retained
1271                in their wrapped form, the originator-usage period and recipient-usage period of
1272                a key-wrapping key is the same. In other cases, a key-wrapping key may be
1273                retained (i.e., stored) so that the files or messages encrypted by the wrapped keys
1274                may be recovered later. In such cases, the recipient-usage period may be
1275                significantly longer than the originator-usage period of the key-wrapping key, and
1276                cryptoperiods lasting for years may be employed.
1277                Cryptoperiod: The recommended originator-usage period for a symmetric key-
1278                wrapping key that is used to wrap very large numbers of keys over a short period
1279                of time is on the order of a day or week. If a relatively small number of keys are to
1280                be wrapped under a key-wrapping key, the originator-usage period of the key-
1281                wrapping key could be up to two years. In the case of a key-wrapping key that is
1282                used for only a single message or communication session, the cryptoperiod would
1283                be limited to a single communication session. It is recommended that a recipient-
1284                usage period extend no more than three years beyond the end of the originator-
1285                usage period.
1286            9. Public key-transport key
1287                Type Considerations: The cryptoperiod for a public key-transport key is that period
1288                during which the public key may be used to apply the encryption operation to the


                                                        31


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
       December 2025                                                                     Part 1 — General

1289                keys that will be protected during transport (i.e., via automated, online
1290                distribution). When the public key has been certified by a CA, the cryptoperiod for
1291                the public key ends when the notAfter date is reached on the last certificate issued
1292                for the public key.
1293                A public key-transport key can be publicly known. Due to the potential need to
1294                decrypt keys after they have been encrypted for transport, the cryptoperiod of a
1295                public key-transport key may be shorter than that of the corresponding private
1296                key-transport key.
1297                Cryptoperiod: Based on cryptoperiod assumptions for the corresponding private
1298                key, a recommendation for the cryptoperiod of the public key-transport key is no
1299                more than one or two years.
1300            10. Private key-transport key
1301                Type Considerations: A private key-transport key may be used multiple times to
1302                decrypt keys. Due to the potential need to decrypt keys after they have been
1303                encrypted for transport, the cryptoperiod of the private key-transport key may be
1304                longer than the cryptoperiod of the corresponding public key. The cryptoperiod of
1305                the private key is the length of time during which any keys encrypted by the
1306                corresponding public key-transport key need to be decrypted.
1307                Cryptoperiod: Given 1) the volume of information that may be protected by keys
1308                encrypted under the corresponding public key-transport key and 2) an
1309                expectation that the security of the key-storage and use environment will increase
1310                as the sensitivity and/or criticality of the processes for which the key provides
1311                protection increases, a cryptoperiod of no more than two years is recommended
1312                for a private key-transport key. In certain applications (e.g., email) where received
1313                messages are stored and decrypted at a later time, the cryptoperiod of a private
1314                key-transport key may exceed the cryptoperiod of the public key-transport key.
1315            11. Symmetric key-agreement key
1316                Type Considerations: A symmetric key-agreement key may be used in multiple
1317                key-agreement transactions. The cryptoperiod of a symmetric key-agreement key
1318                depends on 1) environmental security factors; 2) the types, formats, and volume
1319                of keys that are established; and 3) the details of the key-agreement algorithm
1320                and protocol employed. A symmetric key-agreement key may be used to establish
1321                symmetric keys (e.g., symmetric data-encryption keys and IVs).
1322                Cryptoperiod: Given an assumption that 1) the cryptographic device meets [FIPS
1323                140-3] requirements and 2) the risk level has been established in conformance
1324                with [FIPS 199], an appropriate cryptoperiod for a symmetric key-agreement key
1325                would be no more than one or two years. In certain applications (e.g., email)
1326                where received messages are stored for decryption at a later time, the recipient-
1327                usage period of the key may exceed the originator-usage period.
1328            12. Public static key-agreement key


                                                        32


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
       December 2025                                                                     Part 1 — General

1329                Type Considerations: A public static key-agreement key may be used in multiple
1330                key-agreement transactions. The cryptoperiod for a public static key-agreement
1331                key is usually the same as the cryptoperiod of the corresponding private static key-
1332                agreement key. When the public key has been certified by a CA, the cryptoperiod
1333                for the public key ends when the notAfter date is reached on the last certificate
1334                issued for the public key.
1335                 Cryptoperiod: The cryptoperiod of a public static key-agreement key may be one
1336                 or two years.
1337            13. Private static key-agreement key
1338                Type Considerations: A private static key-agreement key may be used for multiple
1339                key-agreement transactions.
1340                Cryptoperiod: The cryptoperiod of this key depends on 1) environmental security
1341                factors; 2) the types, formats, and volume of keys that are established; and 3) the
1342                details of the key-agreement algorithm and protocol employed. A private static
1343                key-agreement key may be used to establish symmetric keys (e.g., key-wrapping
1344                keys) or other secret keying material.
1345                Given an assumption that 1) the cryptographic device meets [FIPS 140-3]
1346                requirements and 2) the risk level has been established in conformance with [FIPS
1347                199], an appropriate cryptoperiod for the key would be no more than one or two
1348                years. While the cryptoperiods of the private and public static key-agreement keys
1349                are usually the same, in certain applications (e.g., email) where received messages
1350                are stored and decrypted at a later time, the cryptoperiod of a private static key-
1351                agreement key may exceed the cryptoperiod of the corresponding public static
1352                key-agreement key.
1353            14. Public ephemeral key-agreement key
1354                Type Considerations: A public ephemeral key-agreement key is the public-key
1355                component of an asymmetric key pair that is used in a single key-agreement
1356                transaction.
1357                Cryptoperiod: The cryptoperiod of a public ephemeral key-agreement key ends
1358                immediately after it is used to generate a shared secret that is used to derive
1359                keying material. In some cases, the cryptoperiod of a public ephemeral key-
1360                agreement key may be different for the participants in the key-agreement
1361                transaction. For example, consider an encrypted email application in which the
1362                email sender generates an ephemeral key-agreement key pair and then uses the
1363                key pair to generate a key-encrypting key that is used to encrypt a content
1364                encryption key. For the sender, the cryptoperiod of the public key ends when the
1365                shared secret is generated, and the key-encrypting key is derived. However, for
1366                the email receiver, the cryptoperiod of the sender’s ephemeral public key does
1367                not end until the email is deleted, since the shared secret must be generated, and
1368                the key-encrypting key must be determined each time the email is read.



                                                        33


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

1369            15. Private ephemeral key-agreement key
1370                Type Considerations: A private ephemeral key-agreement key is the private
1371                component of an asymmetric key pair that is used in a single transaction to
1372                establish one or more symmetric keys (e.g., key-wrapping keys) or other secret
1373                keying material.
1374                Cryptoperiod: The cryptoperiod of a private ephemeral key-agreement key is used
1375                to generate a shared secret after which the private ephemeral key shall be
1376                destroyed.
1377            16. Public static encapsulation key
1378               Type Considerations: The cryptoperiod for a public static encapsulation key is the
1379               period during which the public key is used to generate and encapsulate symmetric
1380               keys to be provided to the owner of the static encapsulation/decapsulation key pair
1381               in multiple key-establishment transactions. When the public key has been certified
1382               by a CA, the cryptoperiod for the public key ends when the notAfter date is reached
1383               on the last certificate issued for the public static encapsulation key.
1384               Cryptoperiod: The cryptoperiod of the public static encapsulation key depends on
1385               1) environmental security factors; 2) the types, formats, and volume of keys that
1386               are established; and 3) the details of the algorithm and protocol employed. Given
1387               an assumption that the 1) the cryptographic device meets [FIPS 140-3]
1388               requirements, and 2) the risk level has been established in conformance with [FIPS
1389               199], an appropriate cryptoperiod for the public static encapsulation key would be
1390               no more than one or two years.
1391            17. Private static decapsulation key
1392                Type Considerations: A private static decapsulation key is used by the owner of
1393                the static key pair to decapsulate symmetric keys that were generated and
1394                encapsulated using the corresponding public static encapsulation key.
1395                Cryptoperiod: Due to the potential need to decapsulate a key after the execution
1396                of the key-establishment transaction in which it was received, the cryptoperiod of
1397                the private static decapsulation key may need to be longer than the cryptoperiod
1398                of the corresponding public static encapsulation key. The cryptoperiod of the
1399                private static decapsulation key is the length of time during which any key that
1400                was generated and encapsulated using the corresponding public key needs to be
1401                decapsulated. A cryptoperiod of no more than two years is recommended for a
1402                private static decapsulation key.
1403            18. Public ephemeral encapsulation key
1404                Type Considerations: An ephemeral encapsulation/decapsulation key pair is
1405                generated by the entity that will perform decapsulation, and the public
1406                encapsulation key is provided to the entity that will perform encapsulation. The
1407                encapsulating entity uses the encapsulation key in a single key-establishment



                                                        34


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

1408                transaction to encapsulate a symmetric key to be provided to the decapsulating
1409                entity (i.e., the owner of the ephemeral key pair) as ciphertext.
1410                Cryptoperiod: The cryptoperiod of a public ephemeral encapsulation key ends
1411                immediately after it is used to encapsulate a key to be sent to the owner of the
1412                ephemeral key pair.
1413            19. Private ephemeral decapsulation key
1414                Type Considerations: A private ephemeral decapsulation key is used in a single
1415                key-establishment transaction by the owner of the ephemeral
1416                encapsulation/decapsulation key pair to decapsulate a (ciphertext) symmetric key
1417                received during that transaction.
1418                Cryptoperiod: Due to the potential need to decapsulate a key after the key-
1419                establishment transaction that used the corresponding public ephemeral
1420                encapsulation key, the cryptoperiod of the decapsulation key may be longer than
1421                the cryptoperiod of the corresponding public ephemeral encapsulation key (i.e.,
1422                the duration of the key-establishment transaction). The cryptoperiod of the
1423                private ephemeral decapsulation key is the length of time during which the key
1424                encapsulated by the corresponding public ephemeral encapsulation key needs to
1425                be decapsulated. A private ephemeral decapsulation key shall be destroyed as
1426                soon as possible after use.
1427       •    Data encryption/decryption key
1428            20. Symmetric data-encryption/decryption key (for data in transit)
1429                Type Considerations: A symmetric data-encryption key is used to protect data and
1430                messages in transit during communication sessions. Based primarily on the
1431                consequences of a compromise, a data-encryption key that is used to encrypt
1432                large volumes of data over a short period of time (e.g., for link encryption) should
1433                have a relatively short originator-usage period. An encryption key used to encrypt
1434                less data over time could have a longer originator-usage period. The originator-
1435                usage period of a symmetric data-encryption key applies to the use of that key for
1436                encrypting information (see Sec. 4.3.5).
1437                During the originator-usage period, encryption of the data may be performed
1438                using the data-encryption key. The key shall not be used to perform an encryption
1439                operation on data beyond this period. However, the key may need to be available
1440                to decrypt the protected data beyond the originator-usage period (i.e., the
1441                recipient-usage period may need to extend beyond the originator-usage period).
1442                Cryptoperiod: The originator-usage period recommended for the encryption of
1443                large volumes of data over a short period of time (e.g., for link encryption) is on
1444                the order of a day or week. An encryption key used to encrypt smaller volumes of
1445                data might have an originator-usage period of up to two years. A recipient-usage
1446                period of no more than three years beyond the end of the originator-usage period
1447                is suggested.


                                                        35


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
       December 2025                                                                     Part 1 — General

1448            21. Symmetric data encryption/decryption key (for data at rest)
1449                Type Considerations: A symmetric data-encryption key may be used to protect
1450                data in storage. The originator-usage period of a symmetric data-encryption key
1451                applies to the use of that key to encrypt information for storage. The recipient-
1452                usage-period applies to the retrieval and decryption of the data when needed.
1453                This period may extend over many years.
1454                The amount of information that is encrypted using a single data-encryption key
1455                should be limited so that a compromise of the key does not expose vast amounts
1456                of information (e.g., limited to the encryption of no more than a single file or disk
1457                sector; see Sec. 4.3).
1458                When establishing the recipient-usage period, the length of time during which the
1459                data needs to be available and confidential needs to be considered. If the
1460                recipient-usage period is very long, it may become necessary to re-encrypt the
1461                data using a different data-encryption key and/or stronger algorithm (see Sec.
1462                4.6.5).
1463                Cryptoperiod: The originator-usage period recommended for the encryption of
1464                data should be measured in the amount of data to be encrypted using a single key,
1465                whereas the recipient-usage period needs to be set in accordance with the length
1466                of time for which the data needs to be confidential and/or available.
1467       •    Key storage (for operational, backup, and archive storage)
1468            22. Symmetric key-wrapping/unwrapping key
1469                Type considerations: A symmetric key-wrapping/unwrapping key is used to
1470                protect one or more keys in storage (e.g., other symmetric keys or private
1471                asymmetric keys used for protecting storage or communications). The originator-
1472                usage period of a symmetric key-wrapping key applies to the use of that key for
1473                wrapping keys. The recipient-usage-period applies to the retrieval and
1474                unwrapping of a wrapped key when needed. This period may extend over many
1475                years.
1476                The number of keys that are wrapped using a single key-wrapping key should be
1477                limited so that a compromise of the key does not expose vast numbers of wrapped
1478                keys (see Sec. 4.3).
1479                When establishing the recipient-usage period, the length of time during which the
1480                wrapped keys need to be available needs to be considered (e.g., to decrypt data,
1481                unwrap other keys, protect communications).
1482                Cryptoperiod: The originator-usage period recommended for a symmetric key-
1483                wrapping/unwrapping key should be measured in the number of keys to be
1484                wrapped using a key-wrapping key, whereas the recipient-usage period needs to
1485                be set in accordance with the length of time for which the wrapped keys need to
1486                be available.



                                                        36


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

1487            23. Public key-wrapping key
1488                Type considerations: A public key-wrapping key is used to protect one or more
1489                keys in storage (e.g., symmetric keys or private asymmetric keys used for
1490                protecting storage or communications). The cryptoperiod of a public key-
1491                wrapping key applies to the use of that key for wrapping keys. The number of keys
1492                that are wrapped using a single key-wrapping key should be limited so that a
1493                compromise of the key does not expose vast numbers of wrapped keys (see Sec.
1494                4.3).
1495                Cryptoperiod: The cryptoperiod suggested for a public key-wrapping key should
1496                be measured in the number of keys to be wrapped using a single key-wrapping
1497                key.
1498            24. Private key-unwrapping key
1499                Type considerations: A private key-unwrapping key is used to unwrap keys in
1500                storage that were wrapped by the corresponding public key-wrapping key. The
1501                cryptoperiod of this key applies to the retrieval and unwrapping of a wrapped key
1502                when needed. This period may extend over many years.
1503                When establishing the cryptoperiod, the length of time for which the wrapped
1504                keys need to be available needs to be considered (e.g., to decrypt data, unwrap
1505                other keys, protect communications).
1506                Cryptoperiod: The cryptoperiod of a private key-unwrapping key needs to be set
1507                in accordance with the length of time for which the wrapped keys need to be
1508                available.
1509            25. Public encapsulation key
1510                Type considerations: A public encapsulation key may be used to encapsulate
1511                symmetric keys for storage applications (e.g., a data encryption/decryption key to
1512                be used for protecting stored data or a key-wrapping key to protect other keys).
1513                The cryptoperiod of the public encapsulation key applies to the use of that key for
1514                encapsulating symmetric keys. The number of keys that are encapsulated using a
1515                single encapsulation key should be limited so that a compromise of the key does
1516                not expose vast numbers of encapsulated keys (see Sec. 4.3).
1517                Cryptoperiod: The cryptoperiod suggested for a public encapsulation key should
1518                be measured in the number of keys to be encapsulated using that key.
1519            26. Private decapsulation key
1520                Type Considerations: A private decapsulation key is used by the owner of the
1521                encapsulation/decapsulation key pair to decapsulate symmetric keys that were
1522                encapsulated using the corresponding public encapsulation key.
1523                Cryptoperiod: The cryptoperiod of the private decapsulation key may need to be
1524                longer than the cryptoperiod of the corresponding public encapsulation key. The
1525                cryptoperiod of the private decapsulation key is the length of time during which


                                                        37


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

1526                any key that was encapsulated using the corresponding public key needs to be
1527                decapsulated.
1528       •    Authorization keys
1529            27. Symmetric authorization key
1530                Type Considerations: A symmetric authorization key may be used for an extended
1531                period of time, depending on the resources that are protected and the role of the
1532                entity authorized for access. For this key type, the originator-usage period and the
1533                recipient-usage period are the same. The primary considerations in establishing
1534                the cryptoperiod for a symmetric authorization key include the strength of the
1535                key, the adequacy of the cryptographic method, and the adequacy of the key-
1536                protection mechanisms and procedures.
1537                Cryptoperiod: The cryptoperiod of a symmetric authorization key should be no
1538                more than two years.
1539            28. Private authorization key
1540                Type Considerations: A private authorization key may be used for an extended
1541                period of time, depending on the resources that are protected and the role of the
1542                entity authorized for access. The primary considerations in establishing the
1543                cryptoperiod for a private authorization key includes the strength of the key, the
1544                adequacy of the cryptographic method, and the adequacy of the key-protection
1545                mechanisms and procedures. The cryptoperiod of a private authorization key and
1546                its corresponding public key shall be the same.
1547                Cryptoperiod: Given an expectation that the security of the key-storage and use
1548                environment will increase as the sensitivity and criticality of the authorization
1549                processes increases, the cryptoperiod for a private authorization key should be no
1550                more than two years.
1551            29. Public authorization key
1552                Type Considerations: A public authorization key is the public element of an
1553                asymmetric key pair that is used to verify privileges for an entity that possesses
1554                the corresponding private key.
1555                Cryptoperiod: The cryptoperiod of a public authorization key shall be no more
1556                than two years.
1557   Table 1 provides suggested cryptoperiods for each key type. Longer or shorter cryptoperiods
1558   may be warranted, depending on the application and environment in which the keys will be
1559   used. However, when assigning a longer cryptoperiod, serious consideration should be given
1560   to the risks associated with doing so (see Sec. 4.3.1).




                                                        38


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                             Recommendation for Key Management
       December 2025                                                                                                 Part 1 — General

1561                                           Table 1. Suggested cryptoperiods for key types

                                                                                                     Cryptoperiod
                                 Key Type                                     Originator-Usage
                                                                                                                Recipient-Usage Period
                                                                               Period (OUP)
                                                                  Digital Signatures
         1. Private signature key                                                 1 to 3 years                                −
         2. Public signature-verification key                                         Several years (depending on key size)
                                                                    Authentication
         3. Symmetric authentication key                                           < 2 years                          < OUP + 3 years
         4. Private authentication key                                                                 1 to 2 years
         5. Public authentication key                                                                  1 to 2 years
                                                              Random Bit Generation
         6. Symmetric RBG keys                                                   See SP 800-90A                                −
                                                                    Key Derivation
         7. Symmetric key-derivation key                                          About 1 year                                 −
                                                         Key Establishment (automated)
         8. Symmetric key-wrapping/unwrapping key                                  < 2 years                          < OUP + 3 years
         9. Public key-transport key                                                                   1 to 2 years
         10. Private key-transport key                                                                 < 2 years 16
         11. Symmetric key-agreement key                                                              1 to 2 years 17
         12. Public static key-agreement key                                                           1 to 2 years
         13. Private static key-agreement key                                                         1 to 2 years 18
         14. Public ephemeral key-agreement key                                           One key-agreement transaction
         15. Private ephemeral key-agreement key                                          One key-agreement transaction
         16. Public static encapsulation key                                                            1-2 years
         17. Private static decapsulation key                                                          < 2 years 19
         18. Public ephemeral encapsulation key                                         One key-establishment transaction
         19. Private ephemeral decapsulation key                                        One key-establishment transaction
                                                            Data Encryption/Decryption
         20. Symmetric data encryption/decryption key
                                                                                   < 2 years                          < OUP + 3 years
                        (data in transit)
                                                                                                               Until the data no longer
         21. Symmetric data encryption/decryption key                      Limit to the amount of
                                                                                                             needs to be available and/or
                         (data at rest)                                     data to be encrypted
                                                                                                                      confidential
                                        Key Storage (for operational, backup, and archive storage)


       16
          In certain email applications where received messages are stored and decrypted at a later time, the cryptoperiod of a private key-transport
       key may exceed the cryptoperiod of the public key-transport key.
       17 In certain email applications where received messages are stored and decrypted at a later time, the key’s recipient-usage period key may

       exceed the originator-usage period.
       18
          In certain email applications where received messages are stored and decrypted at a later time, the cryptoperiod of a private static key-
       agreement key may exceed the cryptoperiod of the corresponding public static key-agreement key.
       19 In certain applications where encapsulated data is stored and decapsulated, the cryptoperiod of a private static decapsulation key may

       exceed the cryptoperiod of the corresponding public static encapsulation key.



                                                                            39


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                          Recommendation for Key Management
       December 2025                                                                              Part 1 — General

                                                                                  Cryptoperiod
                            Key Type                          Originator-Usage
                                                                                           Recipient-Usage Period
                                                               Period (OUP)
                                                            Limit to the number of       Until the wrapped keys are
        22. Symmetric key-wrapping/unwrapping key
                                                              keys to be wrapped              no longer needed
        23. Public key-wrapping key                                 Limit to the number of keys to be wrapped
        24.Private key-unwrapping key                              Until the wrapped keys are no longer needed
        25. Public encapsulation key                              Limit to the number of keys to be encapsulated
        26.Private decapsulation key                           Until the encapsulated keys are no longer needed
                                                        Authorization
        27. Symmetric authorization key                                              < 2 years
        28. Private authorization key                                                < 2 years
        29. Public authorization key                                                 < 2 years


1562   4.4. Assurances
1563   When keying material (e.g., keys, IVs, and algorithm parameters) is stored or distributed, it
1564   may pass through unprotected environments. In such cases, specific assurances are required
1565   before the keying material is used to perform normal cryptographic operations.

1566   4.4.1. Assurance of Integrity (Integrity Protection)
1567   Assurance of integrity shall be obtained prior to using all keying material.
1568   At a minimum, assurance of integrity shall be obtained by verifying that the keying material
1569   has the appropriate format and came from an authorized source. Additional assurance of
1570   integrity should be obtained through the proper use of error detection codes, MACs, and
1571   digital signatures to provide assurance that keying material has not changed since it was
1572   generated. . For example, a MAC could be generated on stored key information to provide
1573   assurance that the key information has not changed while in storage, and the MAC could be
1574   stored with the key information for easy access.

1575   4.4.2. Assurance of Algorithm Parameter Validity
1576   Algorithm parameters are used with some algorithms to generate keys, create digital
1577   signatures, and establish keying material. Invalid algorithm parameters could void all
1578   intended security for all entities using the algorithms. Methods for obtaining assurance of
1579   algorithm-parameter validity for digital signature algorithms are provided in [SP 800-89].
1580   Methods for obtaining assurance of algorithm-parameter validity for finite-field and elliptic-
1581   curve discrete-log key-agreement algorithms are provided in [SP 800-56A]. Approved
1582   algorithm parameters for key encapsulation are specified in the relevant algorithm
1583   specification (e.g., [FIPS 203] for ML-KEM and [FIPS 204] for ML-DSA). If a public key is
1584   certified by a CA for these algorithms, the CA could obtain this assurance during the
1585   certification process, and users could obtain assurance by verifying the certificates.


                                                             40


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

1586   Otherwise, the key-pair owner and any relying parties are responsible for obtaining the
1587   assurance.

1588   4.4.3. Assurance of Public-Key Validity
1589   Assurance of public-key validity shall be obtained on all public keys before using them.
1590   Assurance of public-key validity gives the user confidence that the public key is correct. This
1591   reduces the probability of using weak or corrupted keys. Invalid public keys could result in
1592   voiding the intended security, including the security of the operation (e.g., digital signature
1593   generation or key establishment), leaking some or all information from the owner’s private
1594   key, and leaking some or all information about a private key that is combined with an invalid
1595   public key (e.g., when key agreement or public-key encryption is performed). One of several
1596   ways to obtain assurance of public-key validity is for an entity to verify certain mathematical
1597   properties that the public key should have. Another way is to obtain assurance from a trusted
1598   third party (e.g., a CA) that the trusted party validated the properties.
1599   Methods for obtaining assurance of public-key validity for the digital signature algorithms
1600   specified in [FIPS 186-5], [FIPS 204], [FIPS 205], and [SP 800-208] are provided in [SP 800-89].
1601   Methods for obtaining this assurance for finite-field and elliptic-curve discrete-log key-
1602   establishment schemes are provided in [SP 800-56A]. Methods for obtaining assurance of
1603   (partial) public-key validity for RSA key-establishment schemes are provided in [SP 800-56B].
1604   Methods for obtaining public-key validity for a KEM are discussed in the relevant algorithm
1605   specification (e.g., [FIPS 203] for ML-KEM).

1606   4.4.4. Assurance of Private-Key Possession
1607   Assurance of static (i.e., long-term) private-key possession shall be obtained before using the
1608   corresponding static public key. Assurance of public-key validity shall always be obtained
1609   prior to or concurrently with assurance of possession of the private key. Assurance of
1610   possession of the correct private key shall be obtained by the key-pair owner (e.g., ensuring
1611   that it is available and has not been modified before use). Entities that receive a public key
1612   shall obtain assurance that the key-pair owner possesses the private key corresponding to
1613   the received public key.
1614   For specific details regarding assurance of the possession of private key-establishment keys,
1615   see [SP 800-56A], [SP 800-56B], and [SP 800-227]. For specific details regarding assurance of
1616   the possession of private digital signature keys, see [SP 800-89]. For public keys that are
1617   certified by a CA, the CA could obtain this assurance during the certification process.
1618   Otherwise, the owner and relying parties are responsible for obtaining the assurance.

1619   4.4.5. Key Confirmation
1620   Key establishment is the process by which keying material is securely established among
1621   entities for subsequent use, usually between pairs of entities. Key confirmation is a procedure
1622   used to provide assurance that these entities actually share the same keying material. This


                                                        41


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

1623   procedure is highly recommended and can be performed either within a key-establishment
1624   process or external to the key-establishment process. For discussions about key confirmation
1625   performed during automated key establishment, see [SP 800-56A], [SP 800-56B], and [SP 800-
1626   227].

1627   4.5. Compromise of Keys and Other Keying Material
1628   Information protected by cryptographic mechanisms is only secure if the algorithms remain
1629   strong and the keys have not been compromised. It is the responsibility of an owner of a
1630   private asymmetric or secret symmetric key to protect the confidentiality of that key. Key
1631   compromise occurs when the protective mechanisms for a key fail (e.g., the confidentiality,
1632   integrity, or association of the key to its owner fail; see Sec. 5), and the key can no longer be
1633   trusted to provide the required security. Reporting a possible key compromise is the
1634   responsibility of anyone who suspects that a key has been compromised (e.g., the key’s
1635   owner observes that the data protected by that key has been compromised).
1636   When a key is compromised, all use of the key to apply cryptographic protection to
1637   information (e.g., compute a digital signature or encrypt information) shall cease, and the
1638   compromised key shall be revoked (see Sec. 7.3.5). However, the continued use of the key
1639   under controlled circumstances to remove or verify the protections (e.g., to decrypt or verify
1640   a digital signature) may be warranted, depending on the risks of continued use and the
1641   organization’s key management policy (see [SP 800-57p2]). The continued use of a
1642   compromised key shall be limited to processing information that has already been protected.
1643   In this case, the entity that uses the information must be made fully aware of the risks
1644   involved. Limiting the cryptoperiod of the key limits the amount of material that would be
1645   compromised (i.e., exposed) if the key were compromised. Using different keys for different
1646   purposes (e.g., different applications or cryptographic mechanisms) and limiting the amount
1647   of information protected by a single key also achieves this purpose (see Sec. 4.3).

1648   4.5.1. Implications
1649   The compromise of a key has the following implications:
1650       1. The unauthorized disclosure of a key that is used to provide confidentiality protection
1651          (i.e., via encryption) means that all information encrypted by that key could be
1652          determined by unauthorized entities. For example, if a symmetric data-encryption key
1653          is compromised, an unauthorized entity could use the key to decrypt past or future
1654          encrypted information (i.e., the information is no longer confidential among the
1655          authorized entities). In addition, a compromised key could be used by an adversary to
1656          encrypt information of the adversary’s choosing, thus providing false information.
1657            The unauthorized disclosure of a private signature key means that the integrity and
1658            non-repudiation qualities of all data signed by that key are suspect. An unauthorized
1659            party in possession of the private key could sign false information and make it appear
1660            to be valid. If it can be shown that the signed data was protected by other mechanisms
1661            (e.g., physical security) from a time before the compromise, the signature may still


                                                        42


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

1662            have some value. For example, if a signed message was received on day 1 and it was
1663            later determined that the private signing key was compromised on day 15, the
1664            receiver may still have confidence that the message is valid because it was maintained
1665            in the receiver’s possession before day 15. Cryptographic timestamping may also
1666            provide protection for messages signed before the private signature key was
1667            compromised. However, the security provided by these other mechanisms is now
1668            critical to the security of the signature. In addition, the authenticity of the signed
1669            message may be questioned, since the private signature key may have been disclosed
1670            to the message receiver or some other entity who then altered the message in some
1671            way. The disclosure of a CA’s private signature key means that an adversary can create
1672            fraudulent certificates and certificate revocation lists (CRLs).
1673       2. A compromise of the integrity of a key means that the key has been accidentally or
1674          deliberately modified or another key has been substituted. This includes a deletion
1675          (i.e., non-availability) of the key. The substitution or modification of a key used to
1676          provide integrity protection calls into question the integrity of all information
1677          protected by that key.
1678       3. A compromise of a key’s usage or application association means that the key could be
1679          used for the wrong purpose (e.g., for key establishment instead of digital signatures)
1680          or for the wrong application and could result in the compromise of information
1681          protected by the key.
1682       4. A compromise of a key’s association with the owner or another entity means that the
1683          identity of the entity cannot be assured (i.e., one does not know who the entity really
1684          is).
1685       5. A compromise of a key’s association with other information means that there is no
1686          association at all or that the association is with the wrong information. This could
1687          cause cryptographic services to fail, information to be lost, or the security of the
1688          information to be compromised.

1689   4.5.2. Protective Measures
1690   Certain protective measures may be used to minimize the likelihood or consequences of a
1691   key compromise. The following is recommended:
1692       1. Limit the amount of time that a secret symmetric key or asymmetric private key is in
1693          plaintext form;
1694       2. Prevent humans from viewing plaintext secret symmetric keys and asymmetric
1695          private keys;
1696       3. Restrict plaintext secret keys and private keys to physically protected “containers,”
1697          including key generators, key-transport devices, key loaders, cryptographic modules,
1698          hardware security modules (HSMs), and key-storage devices;




                                                        43


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

1699       4. Use integrity checks to ensure that the integrity of a key or its association with other
1700          data has not been compromised (e.g., keys may be wrapped in such a manner that
1701          unauthorized modifications to the wrapped key or the key’s metadata will be
1702          detected);
1703       5. Employ key confirmation to help ensure that the proper key was established (see [SP
1704          800-56A], [SP 800-56B], [SP 800-175B], and [SP 800-227]);
1705       6. Establish an accountability system that keeps track of each access to secret symmetric
1706          keys and asymmetric private keys in plaintext form;
1707       7. Provide a cryptographic integrity check on the key (e.g., using a MAC or digital
1708          signature);
1709       8. Use trusted timestamps for signed data;
1710       9. Destroy keys as soon as they are no longer needed; and
1711       10. Create a compromise-recovery plan, especially in the case of a compromised CA key.
1712   The worst form of key compromise is one that is not detected. Nevertheless, even in this case,
1713   certain protective measures can be taken. A key-management system should be designed to
1714   mitigate the negative effects of a key compromise; the system should be designed so that
1715   the compromise of a single key compromises as little data as possible (see [SP 800-152]). For
1716   example, a single cryptographic key could be used to protect the data of only a single human
1717   entity or a limited number of such entities. Systems often have alternative methods to
1718   authenticate communicating entities that do not rely solely on the possession of keys. The
1719   intent is to avoid building a system with catastrophic weaknesses.
1720   A compromise-recovery plan is essential for restoring cryptographic security services in the
1721   event of a key compromise. A compromise-recovery plan shall be documented and easily
1722   accessible. The plan may be included in a Key-Management Practices Statement (see [SP 800-
1723   57p2]). If not, the Key-Management Practices Statement should reference the compromise-
1724   recovery plan.
1725   Although compromise recovery is primarily a local action, the entire community that uses the
1726   system or equipment is affected by the repercussions. Therefore, compromise-recovery
1727   procedures should include the community at large. For example, recovery from the
1728   compromise of a root CA’s private signature key requires all entities that use the
1729   infrastructure to obtain and install a new trust anchor certificate. Typically, this involves
1730   physical procedures that are expensive to implement, so elaborate precautions to avoid
1731   compromise may be justified.
1732   The compromise-recovery plan should address the following topics:
1733       •    The identification of the personnel to notify and what the notification should contain
1734            (e.g., whether specific keys or the certificate-generation process was compromised);
1735       •    The identification of the personnel to perform the recovery actions;
1736       •    The method for obtaining a new key (i.e., re-keying);


                                                        44


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                  Recommendation for Key Management
       December 2025                                                                      Part 1 — General

1737       •    The inventory of all cryptographic keys (e.g., the location of all keys and certificates in
1738            a system);
1739       •    The education of all appropriate personnel on the compromise-recovery procedures;
1740       •    The policies requiring that key-revocation checking be performed to minimize the
1741            effect of a compromise;
1742       •    The monitoring of the re-keying operations to ensure that all required operations are
1743            performed for all affected keys; and
1744       •    Other compromise-recovery procedures, such as:
1745            o A physical inspection of the equipment,
1746            o The identification of all information that may be compromised as a result of the
1747              incident,
1748            o The identification of all signatures that may be invalid due to the compromise of
1749              a signing key, and
1750            o The distribution of new keying material, if required.

1751   4.6. Guidelines for Cryptographic Algorithm and Key-Size Selection
1752   This section discusses the selection of appropriate algorithms and key sizes to provide
1753   adequate protection for 1) the expected lifetime of a system and 2) any sensitive data
1754   protected by that system during the expected lifetime of the data. Cryptographic algorithms
1755   that provide the security services identified in Sec. 2 are specified or adopted in FIPS and NIST
1756   recommendations. Some of these algorithms specify the use of several key sizes.

1757   4.6.1. Cryptographic Algorithm Security Strengths
1758   Cryptographic algorithms can provide different security strengths, depending on the
1759   algorithm and key size used (whenever keys are required by the algorithm). In previous
1760   versions of this document, the estimated (classical) security strength (s) that an algorithm or
1761   system could provide is defined in terms of the amount of work (i.e., the number of
1762   operations) that is required to break the algorithm or system (i.e., the amount of work to
1763   break the algorithm requires 2s operations of some kind, where s = 112, 128, 192, or 256 bits).
1764   However, cryptanalytic advances (e.g., in factoring algorithms, general discrete-logarithm
1765   attacks, and elliptic-curve discrete-logarithm attacks) and the potential advent of quantum
1766   computing have brought into question the use of a single number to represent the security
1767   afforded by designating the same security strength for different algorithm types. In 2016, a
1768   NIST call for the submission of post-quantum algorithms categorized security strength in
1769   terms of the computational resources needed by an attacker to break a block cipher




                                                        45


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                  Recommendation for Key Management
       December 2025                                                                                      Part 1 — General

1770   algorithm or hash function, essentially considering both classical and quantum computers.
1771   Listed in order of increasing strength, these security categories 20 are:
1772        1. Secure against an attacker that is presumed to lack the resources needed for a key
1773           search of a block cipher algorithm with a 128-bit key (e.g., AES-128).
1774        2. Secure against an attacker that is presumed to lack the resources needed for a
1775           collision search on a 256-bit hash function (e.g., SHA-256, SHA3-256).
1776        3. Secure against an attacker that is presumed to lack the resources needed for a key
1777           search of a block cipher algorithm with a 192-bit key (e.g., AES-192).
1778        4. Secure against an attacker that is presumed to lack the resources needed for a
1779           collision search on a 384-bit hash function (e.g., SHA-384, SHA3-384).
1780        5. Secure against an attacker that is presumed to lack the resources needed for a key
1781           search of a block cipher algorithm with a 256-bit key (e.g., AES-256).
1782                              Table 2. Security strength categories for post-quantum algorithms

                       Security
                                                                 Minimum Attack Cost                 Example
                       Category
                          1              Same as key search on a block cipher with a 128-bit key     AES-128
                          2                Same as collision search on a 256-bit hash function       SHA-256
                          3              Same as key search on a block cipher with a 192-bit key     AES-192
                          4                Same as collision search on a 384-bit hash function      SHA3-384
                          5              Same as key search on a block cipher with a 256-bit key     AES-256

1783   The use of strong cryptographic algorithms is critical for cryptographic security. However,
1784   their implementation and use are also of vital concern, since algorithms may unintentionally
1785   be implemented in a manner that leaks information about the key.
1786   The approved symmetric-key encryption algorithms (e.g., AES and Ascon-AEAD128) and
1787   public-key (i.e., asymmetric-key) algorithms require the use of cryptographic keys. Security-
1788   strength estimates were made under the assumption that the keys are a specified length and
1789   are generated and handled in accordance with specific rules (e.g., the keys are generated
1790   using RBGs that were seeded with sufficient entropy and meet certain criteria). However,
1791   these rules are often not followed, and the security provided to the data protected by those
1792   keys may be somewhat less than the security-strength estimates provided (see Sec. 4.6.2).
1793   Section 4.6.1.1 discusses the maximum security strengths that can be provided by the
1794   approved symmetric-key algorithms (e.g., used for encryption and message authentication).
1795   Section 4.6.1.2 discusses the maximum security strengths that can be provided by the
1796   classical asymmetric-key algorithms, while Sec. 4.6.1.3 addresses the security strengths for
1797   the approved quantum-resistant asymmetric-key algorithms. Section 4.6.1.4 discusses the
1798   security strengths of hash functions, XOFs, and their applications.




       20 Appendix C discusses the use of security categories.




                                                                          46


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                            Recommendation for Key Management
       December 2025                                                                                                Part 1 — General

1799   4.6.1.1. Security Strengths of Approved Symmetric-Key Algorithms
1800   Table 3 provides the estimated maximum security strengths for symmetric-key algorithms
1801   and their key lengths.
1802        1. Column 1 indicates the security category (see Table 2).
1803        2. Column 2 indicates the estimated maximum classical security strength (in bits)
1804           provided by the algorithms and key lengths listed in a particular row. The security
1805           strength is not necessarily the same as the length of the key due to attacks on
1806           algorithms that provide computational advantages.
1807        3. Column 3 identifies the symmetric-key algorithms that can provide the security
1808           strength indicated in columns 1 or 2.
1809                                       Table 3. Security strengths of symmetric algorithms

                                                          Classical Security
                     Security Strength (by
                                                              Strength                        Symmetric-Key Algorithms
                          category)
                                                         (by number of bits)
                                                                 ≤ 80                                  2TDEA
                                                                 112                                  3TDEA 21
                                  1                              128                         AES-128 and Ascon-AEAD128
                                  3                              192                                  AES-192
                                  5                              256                                  AES-256

1810   The Triple Data Encryption Algorithm (TDEA) is specified in [SP 800-67] with three keys. 2TDEA
1811   is TDEA with two of the three keys being identical, and 3TDEA is TDEA with three different
1812   keys. As indicated by the orange background in Table 2, 2TDEA and 3TDEA are disallowed for
1813   applying cryptographic protection (e.g., using encryption) but may be used to process already
1814   protected information (e.g., using decryption). See [SP 800-131A] for more detailed
1815   information.
1816   AES is specified in [FIPS 197] with three key sizes: 128, 192, and 256 bits. AES is used with the
1817   modes of operation specified in the [SP 800-38] series of publications.
1818   Ascon-AEAD128 is specified for constrained devices in [SP 800-232] with a single 128-bit key
1819   size.

1820   4.6.1.2. Security Strengths of Approved Classical Asymmetric-Key Algorithms
1821   Table 4 provides the estimated maximum security strengths for the approved classical (i.e.,
1822   non-quantum-resistant) asymmetric-key algorithms and their key lengths. The algorithms
1823   listed in this section do not satisfy the requirements for category 1 (see Table 2), since they




       21 Although 3TDEA is listed as providing 112 bits of security strength, its use is now disallowed for applying cryptographic protection (see

       [SP 800-131A]).



                                                                           47


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                       Recommendation for Key Management
       December 2025                                                                           Part 1 — General

1824   can be broken with a quantum computer with relatively few resources compared to those
1825   needed for a key search on a 128-bit block cipher.
1826       1. Column 1 indicates the estimated maximum classical security strength (in bits)
1827          provided by the algorithms and key lengths listed in a particular row.
1828       2. Column 2 indicates the minimum size of the parameters associated with DSA, as
1829          specified in [FIPS 186-4].
1830       3. Column 3 indicates the minimum size of the parameters associated with the standards
1831          that use finite-field cryptography (FFC) for key agreement. Examples of such
1832          algorithms include Diffie-Hellman (DH) and MQV key agreement, as defined in [SP
1833          800-56A], where L is the size of the public key, and N is the size of the private key.
1834       4. Column 4 indicates the value for k (the size of the modulus n) for algorithms based on
1835          integer-factorization cryptography (IFC). The predominant algorithm of this type is the
1836          RSA algorithm, which is approved in [FIPS 186-5] for digital signatures and in [SP 800-
1837          56B] for key establishment. The value of k is commonly considered to be the key size.
1838       5 Column 5 indicates the range of f (the size of n, where n is the order of the base point
1839         G) for algorithms based on elliptic-curve cryptography (ECC) that are specified for
1840         digital signatures in [FIPS 186-5] and for key establishment in [SP 800-56A]. The value
1841         of f is commonly considered to be the key size.
1842            Table 4. Security strengths of classical (non-quantum-resistant) asymmetric-key algorithms

                    Classical
                    Security                                                               ECC
                                                           FFC          IFC
                  Strength (by           FFC                                        (ECDSA, EdDSA, DH,
                                                        (DH, MQV)      (RSA)
                   number of             DSA                                              MQV)
                      bits)
                                       L = 1024          L = 1024
                      ≤ 80                                            k = 1024          f = 160-223
                                       N = 160           N = 160
                                       L = 2048          L = 2048
                      112                                             k = 2048          f = 224-255
                                       N = 224           N = 224
                                       L = 3072          L = 3072
                      128                                             k = 3072          f = 256-383
                                       N = 256           N = 256
                                       L = 7680          L = 7680
                      192                                             k = 7680          f = 384-511
                                       N = 384           N = 384
                                      L = 15360         L = 15360
                      256                                            k = 15360            f = 512+
                                       N = 512           N = 512

1843   For FFC and IFC algorithms, the listed key sizes do not necessarily match the key sizes
1844   approved in the source documents (i.e., [FIPS 186-5], [SP 800-56A], and [SP 800-56B]). That
1845   is, some key sizes may not be listed, or additional key sizes may be provided with their
1846   associated security strengths. However, estimated security strengths for FFC, IFC, and ECC
1847   algorithms may also be calculated using a formula in [IGD_B].
1848   The algorithm/key-size combinations shown in orange in Table 4 are no longer approved for
1849   applying cryptographic protection on Federal Government information (e.g., generating a
1850   digital signature). However, some flexibility is allowed for processing information that is


                                                             48


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                        Recommendation for Key Management
       December 2025                                                                            Part 1 — General

1851   already protected at those security strengths (e.g., verifying digital signatures) if the receiving
1852   entity accepts the risks associated with doing so. See [SP 800-131A] for more detailed
1853   information.

1854   4.6.1.3. Security Strengths of Approved Quantum-Resistant Asymmetric-Key Algorithms
1855   Table 5 provides the security categories for approved quantum-resistant asymmetric-key
1856   algorithms and their parameter sets.
1857       1. Column 1 indicates the security category associated with the algorithms and
1858          parameter sets listed in a particular row.
1859       2. Column 2 indicates the parameter sets for the ML-KEM key-encapsulation mechanism
1860          specified in [FIPS 203].
1861       3. Column 3 indicates the parameter sets for the ML-DSA digital signature algorithm
1862          specified in [FIPS 204].
1863       4. Column 4 indicates the parameter sets for the SLH-DSA digital signature algorithm
1864          specified in [FIPS 205].
1865       5. Column 5 indicates the parameter sets for the LMS, HSS, XMSS, and XMSSMT digital
1866          signature algorithms in [SP 800-208].
1867                 Table 5. Security categories for the quantum-resistant asymmetric-key algorithms

         Security         ML-KEM              ML-DSA               SLH-DSA               LMS, HSS, XMSS, and
         Category                                                                              XMSSMT
                                                              SLH-DSA-SHA2-128s
                                                             SLH-DSA-SHAKE-128s
             1          ML-KEM-512
                                                              SLH-DSA-SHA2-128f
                                                             SLH-DSA-SHAKE-128f
             2                               ML-DSA-44
                                                                                         LMOTS_SHA256_N24
                                                                                           LMS_SHA256_M24
                                                              SLH-DSA-SHA2-192s
                                                                                          LMOTS_SHAKE_N24
                                                             SLH-DSA-SHAKE-192s
             3          ML-KEM-768           ML-DSA-65                                      LMS_SHAKE_M24
                                                              SLH-DSA-SHA2-192f
                                                                                            WOTSP with n=24
                                                             SLH-DSA-SHAKE-192f
                                                                                             XMSS with n=24
                                                                                          XMSSMT with n=24
                                                                                         LMOTS_SHA256_N32
                                                                                           LMS_SHA256_M32
                                                              SLH-DSA-SHA2-256s
                                                                                          LMOTS_SHAKE_N32
                                                             SLH-DSA-SHAKE-256s
             5         ML-KEM-1024           ML-DSA-87                                      LMS_SHAKE_M32
                                                              SLH-DSA-SHA2-256f
                                                                                            WOTSP with n=32
                                                             SLH-DSA-SHAKE-256f
                                                                                             XMSS with n=32
                                                                                          XMSSMT with n=32




                                                           49


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                           Recommendation for Key Management
       December 2025                                                                                               Part 1 — General

1868   4.6.1.4. Security Strengths of Hash Functions, XOFs, and Their Applications
1869   Cryptographic hashing is the process of transforming an input bit string of arbitrary length
1870   into an output hash value. Hash functions produce hash values of a fixed length, and XOFs
1871   produce a variable-length output (see Sec. 3.1). These approved hashing methods satisfy the
1872   following properties:
1873        1. (Preimage-resistant) It is computationally infeasible to find an input that maps to any
1874           pre-specified output.
1875        2. (Collision-resistant) It is computationally infeasible to find any two distinct inputs that
1876           map to the same output.
1877   The security strength of hash functions and XOFs is determined by the properties required by
1878   the application in which it is used. The appropriate hashing method should be determined by
1879   the algorithm, scheme, or application in which hashing is used and by the minimum security-
1880   strength to be provided. For these applications, a cryptographic key is associated with the
1881   application and needs to be considered when determining the security strength afforded by
1882   the application.22 For example, when generating digital signatures, the minimum key length
1883   for the keys for a given security strength is provided in the FFC, IFC, and ECC columns of Table
1884   4 in Sec. 4.6.1.2, while for HMAC, the key lengths are discussed at
1885   https://csrc.nist.gov/projects/hash-functions.
1886   Table 6 lists the approved hash functions and XOFs specified in [FIPS 180], [FIPS 202], and [SP
1887   800-232], as well as various applications that require collision resistance (e.g., digital
1888   signatures), HMAC, KMAC, key derivation, and random bit generation.
1889        1. Column 1 indicates the estimated maximum classical security strength (in bits)
1890           provided by the algorithms and (if appropriate) the key lengths for a particular row.
1891        2. Column 2 indicates the security category (see Table 2).
1892        3. Column 3 lists the hash functions and XOFs in [FIPS 180], [FIPS 202], and [SP 800-232]
1893           that provide the security strength identified in column 1 and/or column 2 for
1894           applications that require collision resistance (e.g., digital signatures).
1895        4. Column 4 lists the hash functions and XOFs that may be used in the hash-based
1896           applications that meet the security strength identified in column 1 and/or column 2.
1897           These applications require pre-image resistance.
1898   Notes about table entries are provided below the table and are indicated as superscripts
1899   within the table. For these applications, a cryptographic key is associated with the application
1900   and should be considered when determining the security strength afforded by the
1901   application. For example, for the generation of digital signatures, the minimum key length for
1902   the keys for a given security strength is provided in the FFC, IFC, and ECC columns of Table 4
1903   in Sec. 4.6.1.2, while for HMAC, the key lengths are discussed at
1904   https://csrc.nist.gov/projects/hash-functions.

       22 The cryptoperiod for a symmetric key includes both the originator-usage period and the recipient-usage period (see Sec. 4.3.5). Only the

       originator-usage period is terminated.



                                                                           50


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                            Recommendation for Key Management
       December 2025                                                                                Part 1 — General

1905               Table 6. Maximum security strengths for hash functions, XOFs, and their applications

                                                                                                  HMAC,1 KMAC,2
            Classical Security                                     Applications Requiring         Key Derivation
                                         Security Strength
                Strength                                            Collision Resistance           Functions,3
                                           (by category)
           (by number of bits)                                    (e.g., digital signatures)       Random Bit
                                                                                                   Generation4

                   ≤ 805                                                   SHA-16

                                                                      SHA-224,7 SHA-
                   112                                                  512/224,7
                                                                        SHA3-2247

                                                 1                                                    SHA-17

                                                                  SHA-256, SHA-512/256,
                   128                                                                           SHAKE1288, Ascon-
                                                                  SHA3-256, SHAKE1288,
                                                                                                      hash256,
                                                 2                Ascon-hash256, Ascon-
                                                                                               Ascon-XOF128,10 Ascon-
                                                                     XOF128,9 Ascon-
                                                                                                     CXOF12810
                                                                        CXOF1289
                                                                                                  SHA-224,7 SHA-
                                                 3
                 192-224                                                                        512/224,7 SHA3-2247
                                                 4                  SHA-384, SHA3-384
                                                                                               (SHA-256, SHA-512/256,
                                                                                                  SHA-384, SHA-512,
                                                                    SHA-512, SHA3-512,
                  ≥ 256                          5                                               SHA3-256, SHA3-384,
                                                                       SHAKE25611
                                                                                                    SHA3-512),12
                                                                                                     SHAKE25611


1906   Notes for Table 6:
1907       1. HMAC uses a hash function and a key. The security strength provided by HMAC
1908          depends on the hash function used and the length of the key. The estimated security
1909          strength assumes that the length of the key and the security strength provided by the
1910          key-generation process are at least equal to the desired HMAC security strength (in
1911          bits). The HMAC specification [SP 800-224] requires the length of the key to be at least
1912          128 bits.
1913       2. KMAC is based on the KECCAK function specified in [FIPS 202] and uses a key. The
1914          estimated security strength provided by KMAC assumes that the length of the key and
1915          the security strength provided by the key-generation process are at least equal to the
1916          desired security strength (in bits).
1917       3. The security strength for key-derivation assumes that the shared secret computed
1918          during a key-agreement process or the preexisting key-derivation key can support the
1919          desired security strength.
1920       4. The security strength for random bit generation assumes that the RBG has been
1921          instantiated with sufficient randomness to support the desired security strength.


                                                             51


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
       December 2025                                                                     Part 1 — General

1922       5. Applications that provide less than 112 bits of security strength (i.e., ≤ 80, as shown
1923          in orange in Table 6) are not approved for applying cryptographic protection for
1924          Federal Government information (e.g., generating a digital signature or MAC value).
1925          However, some flexibility is allowed for processing information that is already
1926          protected at that security strength (e.g., verifying digital signatures or MAC values) if
1927          the receiving entity accepts the risks associated with doing so. See [SP 800-131A] for
1928          more detailed information.
1929       6. SHA-1 has been demonstrated to provide less than 80 bits of security for digital
1930          signatures.
1931       7. SHA-1 and the 224-bit hash functions (highlighted in yellow in Table 6) are deprecated
1932          for use through 2030 and disallowed for applying cryptographic protection thereafter
1933          (see [SP 800-131A]).
1934       8. The security provided by SHAKE128 depends on the size of the output
1935          (output_length). To obtain 128 bits of collision resistance (see column 3 in Table 6), at
1936          least 256 bits of output are required. For outputs less than 256 bits, the security
1937          strength provided is output_length/2. To obtain 128 bits of pre-image resistance (see
1938          column 4), at least 128 bits of output are required. For outputs less than 128 bits, the
1939          security strength provided is the output_length.
1940       9. A collision resistance security strength of 128 bits is provided by Ascon-XOF128 and
1941          Ascon-CXOF128 only if the length of the output is at least 256 bits.
1942       10. A pre-image security strength of 128 bits is provided by Ascon-XOF128 and Ascon-
1943           CXOF128 only if the length of the output is at least 128 bits.
1944       11. The security provided by SHAKE256 also depends on the size of the output
1945           (output_length). To obtain 256 bits of collision resistance (see column 3 in Table 6), at
1946           least 512 bits of output are required. For outputs less than 512 bits, the security
1947           strength provided is output_length/2. To obtain 256 bits of pre-image resistance (see
1948           column 4), at least 256 bits of output are required. For outputs less than 256 bits, the
1949           security strength provided is the output_length.
1950       12. SHA-256, SHA-512/256, SHA-384, SHA-512, SHA3-256, SHA3-384, and SHA3-512 are
1951           category 5 if the key is at least 256 bits long and is generated at a security strength of
1952           at least 256 bits.

1953   4.6.2. Using Algorithm Suites and the Effective Security Strength
1954   Many applications require multiple cryptographic services (e.g., key establishment,
1955   confidentiality protection, integrity protection, and/or source authentication). A different
1956   algorithm and key could be used to provide each service (e.g., AES could be used for data
1957   encryption, and ML-DSA could be used to generate digital signatures for integrity protection),
1958   or multiple services could be provided by the same algorithm using the same or different keys
1959   (e.g., source authentication and integrity protection could be performed using ML-DSA to
1960   generate digital signatures).


                                                        52


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

1961   Many services can also be provided by more than one algorithm (e.g., key establishment can
1962   be provided by RSA, DH, or ML-KEM). When several algorithms can be used to perform the
1963   same service, some algorithms are inherently more efficient because of their design (e.g., the
1964   use of both HMAC and digital signatures can provide integrity protection, but HMAC is
1965   designed to be more efficient).
1966   In many cases, a variety of key sizes may be available for an algorithm. For some of the
1967   algorithms (e.g., public-key algorithms, such as RSA), the use of key sizes that are larger than
1968   required may impact operations (e.g., larger keys may take longer to generate, require more
1969   memory and transmission bandwidth, and take longer to process data). However, the use of
1970   key sizes that are too small may not provide adequate security.
1971   When selecting a block-cipher cryptographic algorithm (e.g., AES), the block size may also be
1972   a factor to consider, since the amount of security provided by several of the modes defined
1973   in the [SP 800-38] series depend on the block size. Algorithms of different strengths and key
1974   sizes may be used together for performance, availability, or interoperability reasons if
1975   sufficient protection is provided to the data to be protected. In general, the strength of
1976   cryptographic protection is determined by the weakest algorithm and key size used to provide
1977   the protection. A determination of the actual strength of the protection provided for data
1978   includes an analysis of not only the algorithms and key sizes used to apply cryptographic
1979   protections to the information but also the details of how the key and its predecessors were
1980   generated (e.g., the security strength supported by the RBG used during the generation of
1981   the key) and how the key was handled subsequent to generation.
1982   The handling of a key includes any processes that operated on the key (e.g., the key was used
1983   as input to some cryptographic operation). If a key that has been generated to provide a
1984   security strength of s bits is operated upon by a process that has a security strength of less
1985   than s bits, the key is reduced to the security strength of that process. For example, if a key
1986   has been generated by an RBG that provides output with a security strength of 256 bits, then
1987   the key and algorithm combination can provide 256 bits of security strength when the key is
1988   used with AES-256. However, if the key is wrapped using AES-128 (which can provide a
1989   maximum of only 128 bits of security strength), the security strength that can be provided by
1990   the 256-bit key is reduced to 128 bits, even though it is still used with AES-256.
1991   The following is a list of several algorithm combinations and discussions on the security
1992   implications of the algorithm/key-size combination:
1993       1. When a key-establishment scheme is used to establish keying material for use with
1994          one or more symmetric-key algorithms (e.g., AES or HMAC), the security strength that
1995          can be supported by the keying material is determined by the weakest algorithm and
1996          key size used. For example, if ML-KEM 512 is used to establish a 256-bit AES key, no
1997          more than 128 bits of security can be provided for any information protected by that
1998          AES key, since the ML-KEM 512 key can only provide a maximum of 128 bits of security
1999          strength (see Table 5 in Sec. 4.6.1.3).
2000       2. When a hash function and digital signature algorithm are used in combination to
2001          compute a digital signature, the security strength of the signature is determined by


                                                        53


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
       December 2025                                                                     Part 1 — General

2002            the weaker of the two algorithms. For example, if SHA-256 is used with ML-DSA 87
2003            (which can support a security strength of 256 bits), the combination can provide no
2004            more than the 128 bits of security because SHA-256 can provide only 128 bits of
2005            security strength against collisions (see Table 6 in Sec. 4.6.1.4).
2006       3. When an RBG is used to generate a key for a cryptographic algorithm that is intended
2007          to provide X bits of security, an approved RBG shall be used that supports at least X
2008          bits of security. For example, if AES-128 and its key are intended to provide 128 bits
2009          of security strength, then the RBG needs to support at least 128 bits of security.
2010   To support a given security strength, the combination of algorithms and key sizes must be
2011   carefully selected. For example, if 128 bits of security strength is required to protect data that
2012   is to be communicated and provided with confidentiality protection, integrity protection, and
2013   source authentication, the following selection of algorithms and key sizes may be
2014   appropriate:
2015       a. Select an RBG that supports at least a 128-bit security strength to generate keys.
2016       b. Confidentiality: Encrypt the information using AES-128 and a key generated by the
2017          RBG in item a. Other AES key sizes would also be appropriate, but performance may
2018          be a little slower, and generating keys longer than 128 bits would not increase the
2019          amount of security provided.
2020       c. Integrity protection and source authentication: If only one cryptographic operation is
2021          preferred, use digital signatures. Select an algorithm for digital signatures from what
2022          is available to an application (e.g., ECDSA with at least a 256-bit key). SHA-256 or a
2023          larger hash function could be used to hash the data before generating the signature
2024          using the key. If more than one algorithm and key size is available, the selection may
2025          be based on algorithm performance, memory requirements, or other factors as long
2026          as the minimum requirements are met.
2027       d. Key establishment: Select a key-establishment scheme that is based on the
2028          application and environment (see [SP 800-56A], [SP 800-56B], and [FIPS 203]), the
2029          availability of an algorithm in an implementation, and its performance. Select a key
2030          size from Table 4 or Table 5 for an algorithm and key size that can provide at least 128
2031          bits of security. For example, ML-KEM 512 might be a good choice if available.
2032   Agencies that procure systems should consider the potential operational lifetime of the
2033   system. The agencies shall either select algorithms and key sizes that are expected to be
2034   secure during the entire system lifetime or should ensure that the algorithms and key sizes
2035   can be easily updated.

2036   4.6.3. Projected Algorithm and Security Strength Approval Status
2037   Over time, cryptographic algorithms and their associated algorithm parameters (e.g., key
2038   lengths) may become more vulnerable to successful attacks, requiring a transition to stronger
2039   algorithms or longer key lengths. [SP 800-131A] provides the approval status of the NIST-
2040   approved cryptographic algorithms and their key lengths.


                                                        54


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

2041   The minimum security strength required and algorithms to be used during the period in which
2042   specific data must be protected shall be obtained as follows:
2043       •    Determine the security strength required for protecting data during the entire period
2044            of protection.
2045       •    Using [SP 800-131A] and the tables in Sec. 4.6.1, select algorithms and key sizes of the
2046            same or greater strength than required. The security strength is determined not only
2047            by the algorithm used but also by the algorithm parameters and how the key was
2048            generated and handled (see Sec. 4.6.2).
2049       •    When keys are generated for a given algorithm by an RBG, they shall be generated
2050            using RBGs that support the security strength required by the algorithm (e.g., the
2051            design of the RBG and the entropy provided when seeding the RBG).

2052   4.6.4. Transitioning to New Algorithms and Key Sizes in Systems
2053   Advances in computing capabilities, cryptographic research, and cryptanalytic techniques
2054   periodically create the need to replace algorithms or increase key sizes that no longer provide
2055   adequate security for their use cases. For example, the threats posed by future
2056   cryptographically relevant quantum computers to public-key cryptography demand an urgent
2057   migration to quantum-resistant cryptography. Such a transition is costly, takes time, raises
2058   interoperability issues, and disrupts operations.
2059   A system that is designed with a flexible approach to transitioning is recommended. This is
2060   commonly referred to as “crypto agility” and describes the capabilities needed to replace and
2061   adapt cryptographic algorithms to achieve resiliency for protocols, applications, software,
2062   hardware, and infrastructures without interrupting the flow of a running system in order.
2063   Properly designed operational mechanisms that incorporate crypto agility considerations are
2064   needed to facilitate the transition to newer algorithms quickly, smoothly, and without
2065   introducing security breaches or operational disruptions. A more thorough discussion of
2066   crypto agility is available at https://csrc.nist.gov/Projects/crypto-agility.
2067   This section of the document discusses specific algorithm and key size issues. Transitioning to
2068   new cryptographic algorithms and/or key sizes often directly affects enterprise key
2069   management. The impacts can range from minimal changes to accommodate larger key sizes
2070   to large-scale modifications of key types and their supporting key-establishment protocols.
2071   Section 4.6.1 provides estimates of the security strengths that can be supported by the
2072   currently approved classical and quantum-resistant cryptographic algorithms with
2073   recommended key sizes, and [SP 800-131A] provides the current algorithm approval status
2074   information and U.S. Federal Government transition recommendations. These resources can
2075   be used to help plan for transitions from existing cryptographic implementations to those
2076   that meet evolving cryptographic protection requirements. A flexible transition approach
2077   that uses implementations and applications that can most easily be adapted to new
2078   cryptographic security offerings that satisfy evolving requirements is strongly recommended.
2079   This section discusses some of the issues associated with cryptographic transition planning.


                                                        55


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                       Recommendation for Key Management
       December 2025                                                                           Part 1 — General

2080   The estimated time period during which data protected by a specific cryptographic algorithm
2081   (and key size) remains secure is called the algorithm security lifetime. During this lifetime, the
2082   algorithm may be used to both apply cryptographic protection (e.g., encrypt and/or sign data)
2083   and process the protected information (e.g., decrypt and/or verify signed data), although the
2084   period allowed for applying protection could be shorter than the algorithm security lifetime
2085   (see Fig. 2).




2086
2087                       Fig. 2. Algorithm originator-usage period example for symmetric keys

2088   The algorithm is expected to provide adequate protection for the protected data during the
2089   algorithm’s security lifetime. The security lifetime projected when an algorithm is validated
2090   for protecting information may change during its usage period due to newly discovered
2091   cryptographic weaknesses, significant and previously unanticipated increases in cryptanalytic
2092   processing capabilities, or other changes to assumptions that were made when the security
2093   lifetime was projected. Such changes can result in the establishment of an earlier termination
2094   date for the security lifetime of an algorithm’s use for specified functions with a specified key
2095   size.
2096   Typically, an organization selects the cryptographic services that are needed for a particular
2097   application. Then, based on the algorithm security lifetime and the security life of the data
2098   that is to be protected, an algorithm and key-size suite that are sufficient to meet the security
2099   requirements are selected. The organization then establishes a key-management system that
2100   employs validated cryptographic products and provides the services required by the
2101   application. As an algorithm and/or key-size suite nears the end of its security lifetime,
2102   transitioning to a new algorithm and/or key-size suite should be planned with sufficient lead
2103   time to complete the transition within the period for applying cryptographic protection and
2104   the security lifetime of the replaced algorithm suite.
2105   When the algorithm or key size is determined to no longer provide the desired protection for
2106   information (e.g., the algorithm may have been “broken”), any information protected by the
2107   algorithm or key size must be considered suspect (i.e., the data may no longer be confidential,
2108   or the integrity cannot be assured). If the protected data is retained, it should be re-protected
2109   using an approved algorithm and key size that will protect the information for the remainder


                                                           56


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

2110   of its security life. However, it should be assumed that encrypted information that was
2111   transmitted or was otherwise accessed by unauthorized parties during the security suite’s
2112   security lifetime could have been collected and retained for decryption at a later time. Any
2113   plaintext recovered during this period could be used to attempt matched plaintext-ciphertext
2114   attacks on the new algorithm suite following a transition.
2115   When using the tables in Sec. 4.6.1 to select an appropriate algorithm and key size, it is very
2116   important to take the expected security life of the data into consideration as well as the
2117   algorithm/key-size suite’s lifetime. As stated earlier, an algorithm and key size may be used
2118   to both apply cryptographic protection to data and process the protected data. Cryptographic
2119   protection should not be applied to data using a given algorithm and key size if the security
2120   life of the data extends beyond the end of the algorithm security lifetime (i.e., into the time
2121   frame during which the algorithm or key size is disallowed).
2122   For example, using Fig. 2, if the algorithm security lifetime of an encryption algorithm ends
2123   on December 31, 2050, then data with a security life of 10 years should not be encrypted
2124   using that algorithm after December 31, 2040. Instead, a different algorithm should be used
2125   whose lifetime covers the entire security life of the data. If the security life of the data is
2126   longer than originally expected, then the protection provided after 2050 may be less effective
2127   than required, and there is some risk that the confidentiality of the data may be compromised
2128   after 2050.
2129   When implementing cryptographic protection, the strongest algorithm and algorithm-
2130   parameter set (which often includes the key size) that are appropriate to the application
2131   should be used to minimize the frequency of costly transitions. However, performance
2132   considerations also play a part in the selection process. For example, selecting some
2133   algorithms or unnecessarily large key sizes can have adverse performance effects (e.g., the
2134   algorithm may be unacceptably slow, or a communications protocol may not accommodate
2135   a candidate algorithm or key size).
2136   The process of transitioning to a new algorithm or set of algorithm parameters may be as
2137   simple as selecting a more secure option in the set of security suites currently available to a
2138   system, or it can be as complex as building or acquiring an entirely new communications or
2139   authentication system. For example, systems, applications, or communications protocols
2140   currently being procured or in use may not accommodate or be sufficiently flexible to be
2141   configured to accommodate a candidate algorithm suite. The cost, complexity, and time
2142   required for transition can also depend on the scale of the transition. For example,
2143   transitioning to new algorithms and key sizes in a single storage system may have a limited
2144   set of constraints, while changing algorithms and algorithm parameters for implementation
2145   of TLS or SSH across a large enterprise may be much more involved.
2146   The transition to a new cryptographic algorithm is not generally a simple process. The
2147   development of cryptographic modules that satisfy existing and emerging information
2148   communication, processing, and storage requirements need to be developed, subjected to
2149   security testing, and validated as meeting information security standards. Once replacement
2150   cryptography is available, enterprises need to plan and implement changes to or
2151   replacements of cryptographic libraries, implementation validation tools, hardware that


                                                        57


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

2152   implements or accelerates algorithm performance, dependent operating system and
2153   application code, communications devices and protocols, and user and administrative
2154   procedures. If transitioning to newly developed replacement algorithms, cryptographic
2155   implementation standards, procedures, and best practice documentation need to be
2156   changed or replaced, as do installation, configuration, and administration documentation. In
2157   addition, the transitions need to be coordinated among organizations for which mutual
2158   cryptographic interoperability is required.
2159   When a decision is made to replace an algorithm, the time and resources necessary to
2160   implement the new cryptography depends largely on the organization’s readiness to make
2161   the transition in terms of factors such as:
2162        •   Knowing what systems, libraries, and applications employ the algorithm to be
2163            replaced (e.g., a quantum-vulnerable algorithm to be replaced by a quantum-
2164            resistant algorithm)
2165        •   Understanding (by the organization’s management and technicians) the vulnerability
2166            of the organization to exposure or counterfeiting of its information in the event that
2167            specific categories of information are exposed to adversaries or the general public
2168        •   Being able to evaluate the impact of the migration to new cryptography on existing
2169            information technology support contracts and information-sharing agreements
2170        •   Understanding the performance requirements, sources, cost, delivery time frames,
2171            and integration requirements of additional hardware and software that might be
2172            required by changes to performance requirements imposed by differences between
2173            the new algorithms and those of the algorithms currently in use
2174    In order to plan effectively for algorithm transition, an organization will need to understand
2175    the necessity and costs associated with making the transition within specific projected time
2176    frames. Decisions regarding expenditures generally require cost-benefit trade-offs. An
2177    organization cannot be expected to make decisions regarding the expenditure of resources
2178    for future technology transitions without understanding details regarding its readiness for
2179    the transitions. Resource expenditure decisions require quantitative information, and access
2180    to the necessary information requires a measurement of the organization’s readiness for the
2181    initiation and conduct of the transition.
2182    Some specific examples of issues and processes that should be considered when adopting a
2183    new cryptographic algorithm include:
2184       1. The sensitivity of the information over time and the supporting information
2185          system’s lifetime: The sensitivity of the information that will need to be protected by
2186          the system for the lifetime of the new algorithms should be evaluated to determine
2187          the minimum security requirement for the system. Care should be taken not to
2188          underestimate the required lifetime of the system, the sensitivity of the information,
2189          or the period for which the information that it may need to protect will remain
2190          sensitive. Many decisions that were initially considered to be temporary or interim




                                                        58


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)              Recommendation for Key Management
       December 2025                                                                  Part 1 — General

2191            decisions about data sensitivity have since been proven to be inadequate (e.g., the
2192            sensitivity of the data persisted well beyond initial expectations).
2193       2. Algorithm selection: New algorithms should be carefully selected to ensure that they
2194          meet or exceed the security requirements of the system. In general, it is relatively
2195          easy to select cryptographic algorithms and key sizes that offer high security.
2196          However, cryptographic experts should be consulted when making such decisions.
2197          Systems should offer algorithm-suite options that provide for future growth. In the
2198          case of government organizations, the use of validated cryptographic modules using
2199          federally approved algorithms with associated key sizes and operating modes is
2200          required (and recommended for other enterprises).
2201       3. System design: A new system should be designed to meet minimum performance and
2202          security requirements and be sufficiently flexible to accommodate cryptographic
2203          updates. This is often a difficult task, since performance and security goals do not
2204          always align. All aspects of security (e.g., physical security, computer security,
2205          operational security, and personnel security) are involved. If the current system is to
2206          be modified to incorporate new algorithms, the consequences need to be analyzed.
2207          For example, the existing system and the protocols it employs for communication and
2208          storage may require significant modifications to accommodate the “footprints” of the
2209          new algorithms (e.g., key sizes or block sizes). In addition, the security measures
2210          (other than the cryptographic algorithms) that are retained from the system being
2211          upgraded should be reviewed to ensure that they will continue to be effective in the
2212          new system.
2213       4. Pre-implementation evaluation: Strong cryptography may be poorly implemented.
2214          Therefore, changing to new cryptographic techniques should not be done without an
2215          evaluation to consider how effective and secure they are in the target system. In the
2216          case of federal systems, this means validation under [FIPS 140-3].
2217       5. Validation: Algorithms shall be validated through the CAVP and implemented in a
2218          manner consistent with validated CMVP configurations.
2219       6. Installation, operation, and support: Current system installation, operation, and
2220          support requirements should be reviewed for consistency with the suite to which
2221          transition is occurring, and adjustments to relevant plans, procedures, and
2222          documentation should be made and implemented.
2223       7. Testing: All systems should undergo security and functional testing before they are
2224          deployed.
2225       8. Training: If the new system requires new or different tasks (e.g., key-management
2226          procedures) to be performed, then the individuals who will perform those tasks
2227          should be properly trained.
2228       9. System implementation and transition: Care should be taken to implement the
2229          system as closely as possible to the design. Any exceptions should be noted, and
2230          mitigation steps should be identified and scheduled. Implementation considerations



                                                        59


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

2231            should include the means for maintaining the integrity of the algorithm and both the
2232            integrity and confidentiality of cryptographic keys, including physical and logical
2233            protection.
2234       10. Transition: A transition plan should be developed and followed so that the
2235           changeover from the old to the new system runs as smoothly as possible. The plan
2236           should include:
2237                •    An initial phase in which all affected existing cryptographic implementations,
2238                     contractual obligations for affected cryptographic provisioning installation,
2239                     integration maintenance, support, upgrade and remedial change support,
2240                     documentation support, and training support are identified;
2241                •    A phase in which specific transition activities and funding requirements are
2242                     identified and defined; and
2243                •    An implementation phase in which the activities and responsible parties are
2244                     scheduled and assigned.
2245       11. Post-implementation evaluation: The system should be evaluated to verify that the
2246           implemented system meets the system’s security requirements.
2247   For additional considerations associated with a transition to new algorithms, see [CSWP15].

2248   4.6.5. Decrease in Security Over Time
2249   Eventually, the security provided by an algorithm or key may be reduced or lost completely.
2250   For example, the algorithm or key length used may no longer offer adequate security due to
2251   improvements in computational capability or cryptanalysis. In this case, applying protection
2252   to “new” information should be performed using stronger algorithms or keys.
2253   [SP 800-131A] categorizes the approval status of the NIST cryptographic algorithms as
2254   acceptable, deprecated, disallowed, or legacy use. Acceptable and deprecated mean that
2255   an algorithm, scheme, parameter choice, and/or key of a particular type, length, or strength
2256   is approved for use. However, a deprecated status means that there is some security risk in
2257   doing so (e.g., there is less assurance about the amount of security that may be provided by
2258   the cryptographic service).
2259   When a cryptographic service using a cryptographic algorithm, scheme, parameter choice,
2260   and/or key of a particular type, length, or strength is disallowed (i.e., the use of the
2261   cryptographic service using one or more of these cryptographic elements is disallowed), the
2262   assigned cryptoperiod for the key is affected.
2263       •    The cryptoperiod assigned to the private key of an asymmetric key pair that is
2264            disallowed because of one or more of these cryptographic elements shall be
2265            terminated (e.g., a digital signature shall not be generated using any disallowed
2266            cryptographic element). The corresponding public key may continue to be used to
2267            process cryptographically protected information for which the protection was applied



                                                        60


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                           Recommendation for Key Management
       December 2025                                                                                               Part 1 — General

2268              when the cryptographic element was not disallowed (e.g., a signature may be
2269              verified). However, the use of the public key continues to be allowed for legacy use.
2270        •     The originator-usage period assigned to a symmetric key that is disallowed because
2271              of one or more of these cryptographic elements shall be terminated 23 (e.g., data shall
2272              not be encrypted using any disallowed cryptographic element). However, processing
2273              the protected information (e.g., decrypting ciphertext using the key) is allowed for
2274              legacy use.
2275   In either case, a revocation notice should be made available about the revoked key or key
2276   pair. The revocation notice should include a timestamp to indicate the time of the revocation.
2277   There are risks with using a cryptographic service that is only allowed for legacy use. A
2278   reduction in the security that is intended to be provided by an algorithm or key (i.e., as
2279   indicated by an approval status of deprecated or disallowed in [SP 800-131A]) has the
2280   following implications:
2281        •     Encrypted information: The security of encrypted information that was available at
2282              any time to unauthorized entities in its encrypted form should be considered suspect.
2283              For example, keys that were transmitted in encrypted form (e.g., using a key-wrapping
2284              key or key-transport key and an algorithm or key length that is later broken) may need
2285              to be considered compromised, since an adversary could have saved the encrypted
2286              form of the keys for later decryption in case methods for breaking the algorithm are
2287              eventually found (see Sec. 4.5 for a discussion of key compromises). Even if the
2288              transmitted, encrypted information is subsequently re-encrypted for storage using a
2289              different key or algorithm, the information may already be compromised because of
2290              the weakness of the transmission algorithm or key.
2291              Encrypted information that was not “exposed” in this manner (e.g., not transmitted)
2292              may still be secure even though the encryption algorithm or key length no longer
2293              provides the initially required amount of protection. For example, if the encrypted
2294              form of the keys and the information protected by those keys was never transmitted,
2295              then the information may still be confidential.
2296              The lessons to be learned are that an encryption mechanism used for information that
2297              will be available to unauthorized entities in its encrypted form (e.g., via transmission)
2298              should be provided with a high level of security protection, and the use of each key
2299              should be limited (i.e., the cryptoperiod should be short) so that a compromised key
2300              cannot be used to reveal very much information. If the algorithm itself is broken, 24 an
2301              adversary is forced to perform more work to decrypt the information when each key
2302              is used to encrypt a very limited amount of information. Section 4.3.6 discusses
2303              cryptoperiods.




       23 The cryptoperiod for a symmetric key includes both the originator-usage period and the recipient-usage period (see Sec. 4.3.5). Only the

       originator-usage period is terminated.
       24 For example, it is easier to recover a key than to conduct an exhaustive search.




                                                                            61


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                               Recommendation for Key Management
       December 2025                                                                                                   Part 1 — General

2304        •     Digital signatures on stored data that was originally transmitted: 25 Digital signatures
2305              may be computed on data prior to transmission and subsequent storage. In this case,
2306              both the signed data and the digital signature would be stored. If the security provided
2307              by the signature is later reduced (e.g., because of a break of the algorithm or because
2308              an adversary determined the key), the signature may still be valid if the stored data
2309              and its associated digital signature have been adequately protected from modification
2310              since a time prior to the decrease in strength (e.g., by applying a digital signature using
2311              a stronger algorithm or key). 26 Storage capabilities are being developed that employ
2312              cryptographic timestamps to store digitally signed data beyond the normal security
2313              life of the original signature mechanism or its keys (e.g., see [X995] and
2314              [ISO/IEC18014]).
2315        •     Symmetric authentication codes on stored data that was originally transmitted: 27 Like
2316              digital signatures, symmetric authentication codes (i.e., MACs) may be computed on
2317              data prior to transmission and/or subsequent storage. If the received data and
2318              authentication code are stored as received, and the security of the authentication
2319              algorithm or key is disallowed for applying cryptographic protection (e.g., because of
2320              a weakness of the algorithm), the authentication code may still be valid if the stored
2321              data and its associated authentication code have been adequately protected from
2322              modification since a time prior to the disallowance (e.g., by applying another
2323              authentication code using a stronger algorithm or key). 28 Storage capabilities are
2324              being developed that employ cryptographic timestamps to store authenticated data
2325              beyond the normal security life of the original authentication mechanism or its keys
2326              (e.g., see [X995] and [ISO/IEC18014]).
2327




       25 Digital signatures on data that is transmitted but not stored are not considered since their value is short-lived (e.g., the digital signature

       was intended to be used to detect errors introduced only during transmission).
       26
          See Sec. 4.5, item 1 for further discussion.
       27 Symmetric authentication codes on data that is transmitted but not stored are not considered since their value is considered to be short-

       lived.
       28 See Sec. 4.5, item 1 for further discussion.




                                                                             62


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                                Recommendation for Key Management
       December 2025                                                                                                    Part 1 — General

2328   5. Protection Requirements for Key Information
2329   This section provides guidelines regarding the types of protection required for key
2330   information, which includes keying material and associated metadata and varies, depending
2331   on the type of key. The key information must be protected for the security services to be
2332   meaningful. A [FIPS 140-3]-validated cryptographic module may provide much of the
2333   protection needed, depending on its security level. However, additional protection is
2334   required (e.g., by access-control mechanisms in the operating system or by encryption and
2335   integrity protection for key information in an external database) when the key information
2336   exists external to a FIPS 140 cryptographic module. The type of protection needed depends
2337   on the type of key and the security service for which the key is used. [SP 800-152] provides
2338   recommendations for Federal Cryptographic Key Management Systems (FCKMSs) on the
2339   protection of key information when outside a FIPS 140-validated cryptographic module, as
2340   well as other key-management factors to be addressed.

2341   5.1. Protection and Assurance Requirements
2342   Keying material should be (operationally) available for as long as the associated cryptographic
2343   service is required. Keys may be maintained within a cryptographic module while they are
2344   being actively used, or they may be stored externally (provided that proper protection is
2345   afforded) and recalled as needed. Some keys may need to be archived if required beyond the
2346   key’s cryptoperiod (see Sec. 4.3.4 and 4.3.5 for a discussion of the cryptoperiods for
2347   asymmetric and symmetric keys).
2348   The following protections and assurances may be required for the key information:
2349        •     Confidentiality protection shall be provided for all secret key information (e.g.,
2350              symmetric secret keys, asymmetric private keys, key shares, secret metadata). Public
2351              keys, algorithm parameters, and much of the metadata generally do not require
2352              confidentiality protection. Appropriate confidentiality protection is provided when
2353              secret key information exists within a validated cryptographic module that conforms
2354              to [FIPS 140-3] at a security level that is consistent with the [FIPS 199] impact level
2355              associated with the data to be protected by the key (see [SP 800-152]). When the
2356              secret or private key information is available outside of a cryptographic module,
2357              confidentiality protection shall be provided by encryption at an appropriate security
2358              strength (see [SP 800-152]) or by controlling access to the secret key information via
2359              physical means. 29 The security and operational impacts of specific confidentiality
2360              mechanisms vary. Sections 5.2.1.3 and 5.2.2.3 discuss the selection of appropriate
2361              confidentiality mechanisms.
2362        •     Integrity protection shall be provided for all key information. Integrity protection
2363              always requires checking the source and format of the received or retrieved key
2364              information (see Sec. 4.4.1). Appropriate integrity protection is provided when key


       29 This may include storing the secret or private key information in a safe with limited access, logging all access to the key, regularly reviewing

       logs, and using automated alerts when unwarranted access is detected or suspected.



                                                                              63


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
       December 2025                                                                     Part 1 — General

2365            information exists within a validated cryptographic module that conforms to [FIPS
2366            140-3] at a security level that is consistent with the [FIPS 199] impact level associated
2367            with the data to be protected by the key (see [SP 800-152]). When key information is
2368            available outside of a cryptographic module, integrity protection shall be provided by
2369            appropriate cryptographic integrity mechanisms (e.g., cryptographic checksums,
2370            cryptographic hash functions, MACs, or digital signatures), non-cryptographic
2371            integrity mechanisms (e.g., CRCs or parity checks) (see Appendix A), or physical
2372            protection mechanisms. Sections 5.2.1.2 and 5.2.2.2 discuss the selection of
2373            appropriate integrity mechanisms.
2374       •    Association protection shall be provided for a cryptographic security service by
2375            ensuring that the correct keying material is used to protect the correct data in the
2376            correct application or equipment. Sections 5.2.1.4 and 5.2.2.4 discuss the selection of
2377            appropriate association protection methods.
2378       •    Assurance of algorithm-parameter and public-key validity provides confidence that
2379            the parameters and keys used with cryptographic algorithms are correct (see Sec.
2380            4.4.2 and 4.4.3). The selection of appropriate assurance mechanisms is discussed in
2381            [SP 800-56A], [FIPS 203], [FIPS 204], [FIPS 205], [SP 800-89], [SP 800-208], [SP 800-
2382            227], and this document.
2383       •    Assurance of private key possession provides assurance that the owner of a public key
2384            actually possesses the corresponding private key (see Sec. 4.4.4).
2385       •    Availability protection shall be provided for all key information that needs to be
2386            available beyond its immediate use for protecting data (e.g., to decrypt or verify the
2387            continued integrity of the key information). This is accomplished by backing up or
2388            archiving the information (see Sec. 7.2.2.1 and 7.3.1).
2389   The period of protection for key information depends on the type of key, the associated
2390   cryptographic service, and the length of time for which the cryptographic service is required.
2391   The period of protection includes the cryptoperiod of the key (see Sec. 4.3). The period of
2392   protection is not necessarily the same for integrity as it is for confidentiality. Integrity
2393   protection may only be required until a key is no longer used (but not yet destroyed), but
2394   confidentiality protection may be required until the key is actually destroyed.

2395   5.1.1. Summary of Protection and Assurance Requirements for Cryptographic Keys
2396   Table 7 provides a summary of the protection requirements for keys during distribution, key
2397   establishment, and storage. Methods for providing the necessary protection are discussed in
2398   Sec. 5.2.
2399       1. Column 1 (Key Type) identifies the key types.
2400       2. Column 2 (Security Service) indicates the type of security services that are provided
2401          by the key in conjunction with a cryptographic technique. In this column, the word
2402          “support” means that the associated key is used to support the primary cryptographic
2403          services of confidentiality, integrity authentication, and source authentication. For


                                                        64


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                              Recommendation for Key Management
       December 2025                                                                                                  Part 1 — General

2404              example, a key-agreement key may support a confidentiality service by establishing
2405              the key used to provide confidentiality; an RBG key supports the use of cryptography
2406              because it is used to provide the random values for generating the keys to be used to
2407              cryptographically protect information.
2408        3. Column 3 (Security Protection) indicates the type of protection required for the key
2409           (i.e., confidentiality, integrity, and/or availability).
2410        4. Column 4 (Association Protection) indicates the types of associations that need to be
2411           protected for that key, such as associating the key with the usage, application,
2412           authorized communications participants, or other indicated information (e.g., other
2413           keys). The association with algorithm parameters applies only to algorithms where
2414           they are used.
2415        5. Column 5 (Assurances Required) indicates whether the assurance of public-key
2416           validity and/or private-key possession need to be obtained, as defined in [SP 800-
2417           56A], [SP 800-56B], [FIPS 203], [SP 800-89],30 [SP 800-227], and this recommendation.
2418           The assurance of public-key validity provides a degree of confidence that a key is
2419           correct (see Sec. 4.4.3). The assurance of private-key possession provides a degree of
2420           confidence that the entity providing a public key actually possessed the associated
2421           private key at some time (see Sec. 4.4.4).
2422        6. Column 6 (Period of Protection) indicates the length of time for which the
2423           confidentiality, integrity and/or availability of the key needs to be maintained (see
2424           Sec. 4.3). Symmetric keys and private keys shall be destroyed at the end of their
2425           period of protection (see Sec. 7.3.4 and 8.4).
2426                                     Table 7. Protection requirements for cryptographic keys

                                      Security               Security              Association            Assurances        Period of
             Key Type
                                      Service               Protection             Protection              Required        Protection

                                                                   Digital Signatures

                                                                                     Usage or
                                     Source
                                                                                    application
                                  authentication
                                                         Confidentiality             Algorithm                          From generation
        1. Private                   Integrity                                                            Private key
                                                                                    parameters                           until the end of
        signature key             authentication            Integrity 31                                  possession
                                                                                   (when used)                          the cryptoperiod
                                       Non-
                                                                                 Public signature-
                                    repudiation
                                                                                  verification key




       30 SP 800-89 provides information about the assurances provided for digital signatures.
       31 Integrity protection can be provided using a variety of means (see Sec. 4.2.1.2 and 4.2.2.2).




                                                                            65


---

NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                 Recommendation for Key Management
December 2025                                                                                     Part 1 — General

                         Security          Security              Association        Assurances        Period of
     Key Type
                         Service          Protection             Protection          Required        Protection

                                                                  Usage or
                                                                 application
                         Source
                                                               Key-pair owner
                      authentication                                                              From generation
 2. Public                                  Integrity              Algorithm
                         Integrity                                                  Public key         until no
 signature-                                                       parameters
                      authentication      Availability                               validity     signatures need
 verification key                                                (when used)
                          Non-                                                                      to be verified
                                                               Private signature
                       repudiation
                                                                      key
                                                                 Signed data

                                                    Authentication

                                                                  Usage or                        From generation
                         Identity                                application                      until the identity
 3. Symmetric                           Confidentiality
                      authentication                           Other authorized                   or any protected
 authentication                             Integrity
                         Integrity                                 entities                        data no longer
 key                                      Availability
                      authentication                            Authenticated                        needs to be
                                                                    data                           authenticated

                                                                  Usage or
                                                                 application
                         Identity                                   Public
 4. Private           authentication    Confidentiality                                           From generation
                                                                authentication      Private key
 authentication                                                                                    until the end of
                         Integrity          Integrity                key            possession
 key                                                                                              the cryptoperiod
                      authentication                               Algorithm
                                                                  parameters
                                                                 (when used)

                                                                  Usage or
                                                                 application
                                                               Key pair owner
                                                                                                  From generation
                         Identity                               Authenticated
                                                                                                  until the identity
 5. Public            authentication        Integrity               data
                                                                                    Public key    or any protected
 authentication                                                    Private
                         Integrity        Availability                               validity      data no longer
 key                                                            authentication
                      authentication                                                                 needs to be
                                                                     key                           authenticated
                                                                   Algorithm
                                                                  parameters
                                                                 (when used)

                                                 Random Bit Generation

 6. Symmetric                           Confidentiality           Usage or                        From generation
                         Support
 RBG keys                                   Integrity            application                      until replacement




                                                          66


---

NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                 Recommendation for Key Management
December 2025                                                                                     Part 1 — General

                         Security          Security              Association        Assurances        Period of
     Key Type
                         Service          Protection             Protection          Required        Protection

                                                  Key Derivation

                                                                                                  From generation
                                                                  Usage or                         until the end of
 7. Symmetric                                                    application                      the cryptoperiod
                                        Confidentiality
 key-derivation/         Support                               Other authorized                   or the end of the
 master key                                 Integrity              entities                        lifetime of the
                                                                 Derived keys                       derived keys,
                                                                                                  whichever is later

                                         Key Establishment (automated)

                                                                                                  From generation
                                                                  Usage or                         until the end of
 8. Symmetric                           Confidentiality          application                      the cryptoperiod
 key-wrapping                                                                                        or until no
                         Support            Integrity          Other authorized
 key/unwrapping                                                                                     wrapped keys
                                          Availability             entities
 key                                                                                                   require
                                                                Wrapped keys                        unwrapping,
                                                                                                  whichever is later

                                                                  Usage or
                                                                 application                      From generation
 9. Public key-                                                                     Public key
                         Support            Integrity          Key pair owner                      until the end of
 transport key                                                                       validity
                                                                 Private key-                     the cryptoperiod
                                                                transport key

                                                                  Usage or
                                        Confidentiality          application                      From generation
 10. Private key-                                                                   Private key    until no longer
                         Support            Integrity          Encrypted keys
 transport key                                                                      possession       needed to
                                          Availability            Public key-                       decrypt keys
                                                                transport key

                                                                                                  From generation
                                                                  Usage or                         until the end of
 11. Symmetric                          Confidentiality                                           the cryptoperiod
                                                                 application
 key-agreement           Support            Integrity                                             or until no longer
 key                                                           Other authorized                       needed to
                                          Availability             entities                       determine a key,
                                                                                                  whichever is later




                                                          67


---

NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                          Recommendation for Key Management
December 2025                                                                                              Part 1 — General

                              Security             Security              Association          Assurances              Period of
         Key Type
                              Service             Protection             Protection            Required              Protection

                                                                           Usage or
                                                                          application
                                                                                                                 From generation
                                                                       Key pair owner                             until the end of
     12. Public static                                                     Algorithm                             the cryptoperiod
                                                                                               Public key
     key-agreement            Support              Integrity              parameters                             or until no longer
                                                                                                validity
     key                                                                 (when used)                                 needed to
                                                                                                                 determine a key,
                                                                        Private static
                                                                                                                 whichever is later
                                                                       key-agreement
                                                                             key

                                                                           Usage or                              From generation
                                                                          application                             until the end of
     13. Private static                                                    Algorithm                             the cryptoperiod
                                                Confidentiality                                Private key       or until no longer
     key-agreement            Support                                     parameters
                                                   Integrity                                   possession            needed to
     key                                                                 (when used)
                                                                                                                 determine a key,
                                                                       Public static key-                        whichever is later
                                                                        agreement key

                                                                       Key pair owner
                                                                           Private
                                                                       ephemeral key-                             From generation
     14. Public                                                        agreement key                                until the key-
                                                                                               Public key
     ephemeral key-           Support             Integrity 32             Usage or                                  agreement
                                                                                                validity
     agreement key                                                        application                                 process is
                                                                                                                      complete
                                                                           Algorithm
                                                                          parameters
                                                                         (when used)

                                                                           Usage or                               From generation
                                                                          application                              until the end of
                                                                                                                      the key-
                                                                           Public                                    agreement
     15. Private                                Confidentiality        ephemeral key-                                  process
     ephemeral key-           Support
                                                   Integrity           agreement key
     agreement key                                                                                                After the end of
                                                                           Algorithm                              the process, the
                                                                          parameters                                key shall be
                                                                         (when used)                                 destroyed




32 The confidentiality of public ephemeral key-agreement keys may not be protected during transmission. However, the key-agreement
protocols may be designed to detect unauthorized substitutions and modifications of the transmitted public ephemeral keys. In this case,
the protocols form the data integrity mechanism.



                                                                  68


---

NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                Recommendation for Key Management
December 2025                                                                                    Part 1 — General

                         Security          Security             Association        Assurances        Period of
     Key Type
                         Service          Protection            Protection          Required        Protection

                                                                 Usage or
                                                                application
                                                               Key pair owner
 16. Public static                                                                               From generation
                                                               Private static      Public key
 encapsulation           Support            Integrity                                             until the end of
                                                               decapsulation        validity
 key                                                                                             the cryptoperiod
                                                                    key
                                                                 Algorithm
                                                                parameters

                                                                 Usage or
                                                                application
                                                               Encapsulated                       From generation
 17. Private static                     Confidentiality            keys                               until the
                                                                                   Private key
 decapsulation           Support            Integrity           Public static                    encapsulated keys
                                                                                   possession
 key                                      Availability         encapsulation                     no longer need to
                                                                    key                           be decapsulated
                                                                 Algorithm
                                                                parameters

                                                               Key pair owner
                                                                  Private
                                                                ephemeral                        From generation
 18. Public                                                    decapsulation                          until the
 ephemeral                                                          key            Public key      encapsulation
                         Support            Integrity
 encapsulation                                                                      validity         process is
 key                                                             Usage or                          complete for a
                                                                application                      single transaction
                                                                 Algorithm
                                                                parameters

                                                                 Usage or                         From generation
                                                                application                        until the end of
 19. Private                            Confidentiality           Public                         the decapsulation
 ephemeral                                                      ephemeral          Private key         process
                         Support            Integrity
 decapsulation                                                 encapsulation       possession     After the end of
 key                                      Availability             key                            the process, the
                                                                 Algorithm                          key shall be
                                                                parameters                           destroyed

                                           Data Encryption/Decryption

 20. Symmetric                          Confidentiality                                           From generation
 data-encryption/                                                Usage or                          until the end of
                      Confidentiality       Integrity
 decryption key                                                 application                      the lifetime of the
 (data in transit)                        Availability                                           data or the end of




                                                          69


---

NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                 Recommendation for Key Management
December 2025                                                                                     Part 1 — General

                         Security          Security              Association        Assurances        Period of
     Key Type
                         Service          Protection             Protection          Required        Protection

 21. Symmetric                                                 Other authorized                   the cryptoperiod,
 data encryption/                                                  entities                       whichever is later
 decryption key:                                                 Plaintext/
 (data at rest)                                                Encrypted data

                            Key Storage (for operational, backup, and archive storage)

                                                                                                  From generation
                                                                  Usage or                         until the end of
                                        Confidentiality          application                      the cryptoperiod
 22. Symmetric
                                                                                                      or until no
 key-wrapping/           Support            Integrity          Other authorized
                                                                                                    wrapped keys
 unwrapping key                           Availability             entities
                                                                                                      need to be
                                                                Wrapped keys                         unwrapped,
                                                                                                  whichever is later

                                                                  Usage or
                                                                 application                      From generation
 23. Public key-                                                                    Public key
                         Support            Integrity          Key pair owner                      until the end of
 wrapping key                                                                        validity
                                                                 Private key-                     the cryptoperiod
                                                               unwrapping key

                                                                                                  From generation
                                                                  Usage or                         until the end of
                                        Confidentiality          application                      the cryptoperiod
 24.Private key-                                                                    Private key    of the wrapping
                         Support            Integrity           Wrapped keys
 unwrapping key                                                                     possession    key or no further
                                          Availability           Public key-                       keys need to be
                                                                wrapping key                         unwrapped,
                                                                                                  whichever is later

                                                                  Usage or
                                                                 application
                                                               Key pair owner
 25. Public                                                                                       From generation
                                                                   Private          Public key
 encapsulation           Support            Integrity                                              until the end of
                                                                decapsulation        validity
 key                                                                                              the cryptoperiod
                                                                     key
                                                                  Algorithm
                                                                 parameters




                                                          70


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                 Recommendation for Key Management
       December 2025                                                                                     Part 1 — General

                                Security          Security              Association        Assurances        Period of
            Key Type
                                Service          Protection             Protection          Required        Protection

                                                                         Usage or
                                                                        application
                                                                       Encapsulated
                                               Confidentiality             keys                           From generation
        26.Private
                                                                                           Private key   until no keys need
        decapsulation           Support            Integrity              Public           possession           to be
        key                                      Availability          encapsulation                        decapsulated
                                                                           key
                                                                         Algorithm
                                                                        parameters

                                                          Authorization

                                                                         Usage or                        From generation
        27. Symmetric                          Confidentiality          application                       until the end of
        authorization        Authorization
                                                   Integrity          Other authorized                   the cryptoperiod
        keys
                                                                          entities                          of the key

                                                                         Usage or
                                                                        application
                                                                          Public                         From generation
        28. Private                            Confidentiality         authorization       Private key    until the end of
        authorization        Authorization
                                                   Integrity               key             possession    the cryptoperiod
        key
                                                                          Algorithm                       of the key pair
                                                                         parameters
                                                                        (when used)

                                                                         Usage or
                                                                        application
                                                                      Key pair owner
                                                                                                         From generation
        29. Public                                                        Private          Public key     until the end of
        authorization        Authorization         Integrity           authorization        validity     the cryptoperiod
        key                                                                 key                           of the key pair
                                                                          Algorithm
                                                                         parameters
                                                                        (when used)


2427   5.1.2. Summary of Protection Requirements for Other Related Information
2428   Table 8 summarizes the protection requirements for other related information during
2429   distribution and storage. Mechanisms for providing the necessary protection are discussed in
2430   Sec. 5.2.
2431       1. Column 1 (Information Type) identifies the type of information.




                                                                 71


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                           Recommendation for Key Management
       December 2025                                                                                               Part 1 — General

2432        2. Column 2 (Security Service) indicates the type of security service provided by the
2433           information.
2434        3. Column 3 (Security Protection) indicates the type of security protection required for
2435           the information.
2436        4. Column 4 (Association Protection) indicates the relevant types of associations for each
2437           type of information.
2438        5. Column 5 (Assurance of Algorithm Parameter Validity) indicates the information for
2439           which assurance shall be obtained, as defined in [SP 800-56A], [SP 800-56B], [FIPS
2440           203], [FIPS 204], [FIPS 205], [SP 800-208], [SP 800-227], and Sec. 4.4 of this document.
2441           Assurance of algorithm-parameter validity provides confidence that algorithm
2442           parameters are correct.
2443        6. Column 6 (Period of Protection) indicates the length of time for which the integrity
2444           and/or confidentiality of the information need to be maintained. The information
2445           shall be destroyed at the end of the period of protection (see Sec. 7.3.4).
2446                                Table 8. Protection requirements for other related information

                                                                                                           Assurance
                                                                                                               of
         Information              Security               Security                  Association                                   Period of
                                                                                                           Algorithm
             Type                 Service               Protection                 Protection                                   Protection
                                                                                                           Parameter
                                                                                                            Validity
                                Depends on                                   Usage or application                                 From
        Algorithm                 the key                Integrity                                                            generation
                                                                                Private and public              Yes
        parameters            associated with          Availability                                                          until no longer
                              the parameters                                           keys                                     needed
                                                                                                                                  From
                                                                                                                               generation
        Initialization          Depends on              Integrity 33                                                         until no longer
                                                                                 Protected data
        vectors                the algorithm           Availability                                                            needed to
                                                                                                                              process the
                                                                                                                             protected data
        Shared                                                                                                                     From
        secrets                                      Confidentiality                                                           generation
        (generated                Support                                                                                    until the end of
                                                         Integrity
        during key                                                                                                                  the
        agreement)                                                                                                            transaction.




       33 IVs are not generally protected during transmission. However, the decryption system may be designed to detect or minimize the effect

       of unauthorized substitutions and modifications to transmitted IVs. In this case, the decryption system is the data-integrity mechanism.



                                                                           72


---

NIST SP 800-57pt1r6 ipd (Initial Public Draft)                           Recommendation for Key Management
December 2025                                                                               Part 1 — General

                                                                                Assurance
                                                                                    of
  Information         Security           Security              Association                      Period of
                                                                                Algorithm
      Type            Service           Protection             Protection                      Protection
                                                                                Parameter
                                                                                 Validity
                                                                                             When used for
                                                                                                 random
                                                                                                 number
                                                                                               generation,
                                                                                                 destroy
                                                                                              immediately
                                                                                                after use
                                                                                             When used to
                                      Confidentiality    Usage or application                  regenerate
 Seeds                 Support
                                         Integrity                                                keying
                                                                                                material,
                                                                                             destroy when
                                                                                                no longer
                                                                                             needed (e.g.,
                                                                                             at the end of
                                                                                                    the
                                                                                             cryptoperiod
                                                                                               of the keys)
                                                                                                  From
                                                         Usage or application                  generation
                                                             Other authorized                until no longer
 Other public                                                    entities                      needed to
                       Support           Integrity
 information                                                                                  process data
                                                        Data processed using                    using the
                                                              a nonce                             public
                                                                                              information
                                                                                                  From
                                                         Usage or application                  generation
                                                             Other authorized                until no longer
 Other secret                         Confidentiality            entities                      needed to
                       Support
 information                             Integrity      Data processed using                  process data
                                                             the secret                         using the
                                                            information                           secret
                                                                                              information
                                                                                                   From
                                                                                               generation
                                                                                             until no longer
 Intermediate                         Confidentiality                                          needed and
                       Support                           Usage or application
 results                                Integrity                                                   the
                                                                                              intermediate
                                                                                                results are
                                                                                                destroyed




                                                        73


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                          Recommendation for Key Management
       December 2025                                                                              Part 1 — General

                                                                                       Assurance
                                                                                           of
         Information         Security           Security             Association                      Period of
                                                                                       Algorithm
             Type            Service           Protection            Protection                      Protection
                                                                                       Parameter
                                                                                        Validity
                                                                                                        From
        Key-control
                                                Integrity                                            generation
        information
                              Support                                    Key                           until the
        (e.g., IDs,                            Availability
                                                                                                   associated key
        purpose)
                                                                                                    is destroyed
                                                                                                        From
                                             Confidentiality                                         generation
                                              (depends on                                          until no longer
        Random
                              Support            usage)                                             needed, and
        number
                                                                                                    the random
                                                Integrity
                                                                                                     number is
                                                                                                     destroyed
                                                                                                         From
                                                                                                      generation
                             Identity        Confidentiality                                        until replaced
                                                                Usage or application                 or no longer
        Password          authentication        Integrity
                                                                    Owning entity                     needed to
                          Key derivation       Availability                                         authenticate
                                                                                                   the entity or to
                                                                                                     derive keys
                                                Integrity           Audited events                      From
        Audit                                    Access             Key-control                     generation
                              Support         authorization
        information                                            information/metadat                 until no longer
                                               Availability              a                            needed



2447   5.2. Protection Mechanisms
2448   During the lifetime of key information, the key information is either “in transit” (e.g., is in the
2449   process of being distributed manually or using automated protocols to the authorized
2450   communication participants for use by those entities), “at rest” (e.g., the key information is
2451   in storage), or “in use.” In all cases, the key information shall be protected in accordance with
2452   Sec. 5.1.
2453   Keys that are in use shall reside (and be used) within appropriate cryptographic modules. A
2454   key being in use does not preclude that key from also being simultaneously in transit and/or
2455   in storage.
2456   While in transit or in storage, the choice of protection mechanisms may vary. Although
2457   several methods of protection are provided in the following subsections, not all methods
2458   provide equal security. The method should be carefully selected. In addition, the mechanisms
2459   prescribed do not, by themselves, guarantee protection. The implementation and associated



                                                               74


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

2460   key management need to provide adequate security to prevent any feasible attack from being
2461   successful.

2462   5.2.1. Protection Mechanisms for Key Information in Transit
2463   Key information in transit may include keying material that is being distributed to:
2464       •    Obtain a cryptographic service (e.g., to establish a key that will be used to provide
2465            confidentiality) (see Sec. 7.1.5) or
2466       •    Back up or archive key information for possible use or recovery in the future (see Sec.
2467            7.2.2 and 7.3.1) or
2468       •    Recover the backed up or archived key information (see Sec. 7.2.2.2, 7.3.1, and
2469            Appendix B).
2470   This may be accomplished manually (i.e., via a trusted courier), in an automated fashion (i.e.,
2471   using an automated communication protocol), or by some combination of manual and
2472   automated methods. For some protocols, the protections are provided by the protocol; in
2473   other cases, the protection of the key information is provided directly to the key information
2474   (e.g., the keying material is encrypted prior to transmission for decryption only by the
2475   receiving party). It is the responsibility of the originating party to apply protection
2476   mechanisms and the responsibility of the recipient to undo or check the mechanisms used.

2477   5.2.1.1. Availability
2478   Since communications may be garbled, intentionally altered, or destroyed, the availability of
2479   key information after transit cannot be assured using cryptographic methods alone. However,
2480   availability can be supported by redundant or multiple channels, store-and-forward systems
2481   (i.e., deletion by the sender only after confirmation of receipt by the intended recipient),
2482   error correction codes, and other non-cryptographic mechanisms.
2483   Communication systems should incorporate non-cryptographic mechanisms to ensure the
2484   availability of key information after transmission rather than relying on retransmission by the
2485   original sender.

2486   5.2.1.2. Integrity
2487   Integrity protection involves both the prevention and detection of modifications to
2488   information. The absolute prevention of modifications is not possible, but there are
2489   mechanisms (e.g., key-confirmation mechanisms) that can be used to detect modifications
2490   with a high probability. When modifications are detected, measures may be taken to restore
2491   the information to its unaltered form. Cryptographic mechanisms are often used for this
2492   purpose. Assurance of the integrity of received key information shall be obtained using one
2493   or more of the following methods:
2494       1. Manual method (physical protection is provided):


                                                        75


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

2495                a. An integrity mechanism is used to generate a “code” (e.g., CRC, MAC, or digital
2496                   signature) on the key information to be distributed, and the resulting code is
2497                   provided to the recipient along with the key information. If the received code
2498                   is successfully verified by the recipient, then the recipient has assurance that
2499                   the keying material has been received correctly. Otherwise, the key
2500                   information is assumed to be corrupted. When using a cryptographic
2501                   algorithm that uses a key to generate the code (e.g., a MAC or digital
2502                   signature), the sender and recipient must know the appropriate keys for
2503                   generating and verifying the code. A CRC may be used instead of a MAC or
2504                   digital signature to generate the code, since the physical protection is only
2505                   intended to protect against intentional modifications.
2506                     -OR-
2507                b. The key in the key information being distributed is used to perform the
2508                   intended cryptographic operation on mutually known data (e.g., to encrypt
2509                   plaintext data that is known by both the sender and the recipient). Both the
2510                   key information and the cryptographically protected data are sent to the
2511                   recipient. If the recipient can use the received key to successfully reverse or
2512                   verify the cryptographic operation performed by the sender (e.g., by using the
2513                   key to decrypt the received ciphertext data and successfully compare the
2514                   resulting plaintext data with the mutually known plaintext data), the recipient
2515                   has assurance that the key information has been received correctly.
2516                   Otherwise, the key information is assumed to be corrupted.
2517       2. Automated distribution via communication protocols (protection provided by the
2518          sending entity or by the communication protocol):
2519                a. An approved cryptographic integrity mechanism (e.g., a MAC or digital
2520                   signature) is used on the key information to be distributed, and the resulting
2521                   code is provided to the recipient along with the key information for
2522                   subsequent verification to obtain an assurance of integrity. A CRC is not
2523                   approved for this purpose. The integrity mechanism may only be applied to
2524                   the key information or to an entire message.
2525                     -OR-
2526                b. The key in the key information is used by the sender to perform the intended
2527                   cryptographic operation on data (e.g., compute a MAC on the data). Both the
2528                   key information and the cryptographically protected data are sent to the
2529                   recipient. If the recipient can successfully use the key to reverse or verify the
2530                   cryptographic operation (e.g., verify the MAC on the received data using the
2531                   received key), the recipient has assurance that the key information has been
2532                   received correctly. Otherwise, the key information is assumed to be corrupted.
2533   The response to the detection of an integrity failure will vary, depending on the specific
2534   environment. Improper error handling can allow attacks (e.g., side-channel attacks). A
2535   security policy (see [SP 800-57p2]) should define the response to such an event. For example,


                                                        76


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
       December 2025                                                                     Part 1 — General

2536   if an error is detected in the received key information, and the receiver requires the key
2537   information to be entirely correct (e.g., the receiver cannot proceed when the key
2538   information is in error), then:
2539            •   The key information should not be used,
2540            •   The recipient may request that the key information be resent (retransmissions
2541                should be limited to a predetermined maximum number of times), and
2542            •   Information related to the incident should be saved in an audit log to later identify
2543                the source of the error.

2544   5.2.1.3. Confidentiality
2545   Confidentiality protection shall be provided for secret symmetric keys, asymmetric private
2546   keys, key shares, and other secret information (e.g., seeds or secret metadata) during transit
2547   using one or more of the following methods:
2548       1. Manual method:
2549                a. The secret key information is encrypted (e.g., wrapped or encapsulated) using
2550                   an approved technique that provides protection at a security strength that
2551                   meets or exceeds the security strength required for the keying material (i.e.,
2552                   the security strength required for the protection of the data to be protected
2553                   by the key).
2554                     -OR-
2555                b. A key is separated into key shares that are each generated at a security
2556                   strength that meets or exceeds the security strength required by the keying
2557                   material (i.e., the security strength required for the protection of the data to
2558                   be protected by the key). Each key share is handled using split-knowledge
2559                   procedures (see Sec. 7.1.5.2.1 and 7.1.5.2.2.1) so that no single individual can
2560                   acquire access to all key shares. Any metadata that needs to be kept secret is
2561                   encrypted.
2562                     -OR-
2563                c. Appropriate physical and procedural protection is provided (e.g., by using a
2564                   trusted courier).
2565       2. Automated distribution via communication protocols: The secret key information is
2566          encrypted (e.g., wrapped or encapsulated) using an approved technique that provides
2567          protection at the security strength that meets or exceeds the security strength
2568          required of the keying material (i.e., the security strength required for the protection
2569          of the data to be protected by the key).




                                                        77


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
       December 2025                                                                     Part 1 — General

2570   5.2.1.4. Association With Usage or Application
2571   The association of keying material with its usage or application shall either be specifically
2572   identified during the distribution process (e.g., included in the transmitted metadata) or be
2573   implicitly defined by the use of the application. See Sec. 5.2.3 for a discussion of the metadata
2574   associated with keys.

2575   5.2.1.5. Association With Other Entities
2576   The association of keying material with all appropriate entities (e.g., any entity that shares
2577   the keying material) shall either be specifically identified during the distribution process (e.g.,
2578   using public-key certificates) or implicitly defined by the use of the application. See Sec. 5.2.3
2579   for a discussion of the metadata associated with keys.

2580   5.2.1.6. Association With Other Related Key Information
2581   Any association with other related key information (e.g., algorithm parameters, the
2582   encryption/decryption key, IVs) shall either be specifically identified during the distribution
2583   process or implicitly defined by the use of the application. See Sec. 5.2.3 for a discussion of
2584   the metadata associated with the other related key information.

2585   5.2.2. Protection Mechanisms for Key Information in Storage
2586   Key information may be stored in some device or on storage media. This may include copies
2587   of the key information that are also in transit or in use. Stored key information shall be
2588   protected in accordance with Sec. 5.1.
2589   The key information may be stored so that it is immediately available to an application (e.g.,
2590   on a local hard disk or a server). This would be typical for key information stored within a
2591   cryptographic module or in immediately accessible storage (e.g., on a local hard drive). The
2592   key information may also be stored in electronic form on removable media (e.g., a CD-ROM
2593   or flash drive) in a remotely accessible location or in hard copy form and placed in a safe. This
2594   would be typical for backup or archive storage.
2595   Sections 5.2.2.1 through 5.2.2.3 address the availability, integrity, and confidentiality of key
2596   information. Sections 5.2.2.4 through 5.2.2.6 discuss the association of key information with
2597   its usage, application, other entities, and other related key information. Section 5.2.2.7
2598   discusses storage media and mechanisms.

2599   5.2.2.1. Availability
2600   Key information may need to be readily available for as long as data is protected by the key.
2601   A common method for providing this protection is to make one or more copies of the key
2602   information and store them in separate locations. During a key’s cryptoperiod, key
2603   information that requires long-term availability should be stored in both normal operational



                                                        78


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

2604   storage (see Sec. 7.2.1) and in backup storage (see Sec. 7.2.2.1). Key information that is
2605   retained after the end of a key’s cryptoperiod should be placed in archive storage (see Sec.
2606   7.3.1). This recommendation does not preclude the use of the same storage media for both
2607   backup and archive storage.
2608   Specifics on the long-term availability requirement for each key type are addressed for
2609   backup storage in Sec. 7.2.2.1 and for archive storage in Sec. 7.3.1. Some algorithms have
2610   strict requirements on key backup (e.g., the stateful hash-based signatures specified in [SP
2611   800-208]).
2612   The recovery of this key information to replace lost key information (e.g., from normal
2613   storage) or in performing cryptographic operations after the end of a key’s cryptoperiod is
2614   discussed in Sec. 7.2.2.2 (recovery during normal operations), Sec. 7.3.1 (recovery from
2615   archive storage), and Appendix B.
2616   Even though the primary focus of this section is to provide assurance of the availability of key
2617   information, there is at least one example in which denying the availability of this key
2618   information may be desired — namely, when sanitizing large volumes of key information that
2619   have been encrypted. In this case, cryptographic sanitization (i.e., destroying the key used to
2620   decrypt or unwrap the key information) is suggested (see [SP 800-88]).

2621   5.2.2.2. Integrity
2622   Integrity protection is concerned with ensuring that the key information retrieved from
2623   storage is correct. While absolute protection against modification is not possible, reasonable
2624   measures can help prevent unauthorized modifications, detect any modifications that occur
2625   with a very high probability, and restore key information to its original content.
2626   All key information requires integrity protection. Integrity protection shall be provided by
2627   physical mechanisms, cryptographic mechanisms, or both.
2628   Physical mechanisms include the use of:
2629       •    A validated cryptographic module or operating system that limits access to the stored
2630            key information
2631       •    A computer system or media that is not connected to other systems
2632       •    A physically secure environment with appropriate access controls that is outside of a
2633            computer system (e.g., in a safe with limited access)
2634   Cryptographic mechanisms include the use of:
2635       •    An approved cryptographic integrity mechanism (e.g., MAC or digital signature) that
2636            is computed on the key information before placing it in storage and is later used to
2637            verify the integrity of the stored key information and
2638       •    Performing the intended cryptographic operation.
2639   If the retrieved key information is incorrect, the keying material may have been corrupted. In
2640   order to restore the key information when an error is detected, one or more copies of the


                                                        79


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

2641   key information should be maintained in physically separate locations (i.e., in backup or
2642   archive storage; see Sec. 7.2.2.1 and 7.3.1). The integrity of each copy should be periodically
2643   checked.

2644   5.2.2.3. Confidentiality
2645   One of the following mechanisms shall be used to provide confidentiality for key information
2646   that needs to remain secret while in storage:
2647       •    Encryption (or key wrapping or encapsulation) using an approved algorithm in a [FIPS
2648            140-3]-validated cryptographic module. The encryption shall use an approved
2649            technique that provides protection at a security strength that meets or exceeds the
2650            security strength required for the key information.
2651            -OR-
2652       •    Physical protection provided by a [FIPS 140-3]-validated cryptographic module at a
2653            security level that is consistent with the [FIPS 199] impact level associated with the
2654            data to be protected by the key (see [SP 800-152]).
2655            -OR-
2656       •    Physical protection provided by secure storage with controlled access (e.g., a safe or
2657            protected area).

2658   5.2.2.4. Association With Usage or Application
2659   Keying material is used with a given cryptographic mechanism (e.g., to generate a digital
2660   signature or establish keys) or with a particular application. Protection shall be provided to
2661   ensure that the keying material is not used incorrectly (e.g., not only must the usage or
2662   application be associated with the keying material, but the integrity of this association must
2663   be maintained). This protection can be provided by separating the keying material from that
2664   of other mechanisms or applications or by using appropriate metadata associated with the
2665   keying material. Section 5.2.3 addresses the metadata associated with keys.

2666   5.2.2.5. Association With the Other Entities
2667   Some key information needs to be correctly associated with another entity (e.g., the key
2668   source or an entity that owns or uses the key), and the integrity of this association shall be
2669   maintained. For example, a secret symmetric key used for the encryption of key information
2670   or the computation of a MAC needs to be associated with other entities that share the key.
2671   Public keys need to be correctly associated (e.g., cryptographically bound) with the owner of
2672   the key pair (e.g., using public-key certificates).
2673   The key information shall retain its association during storage by separating the key
2674   information by entity, application, or by using appropriate metadata for the key information
2675   when required. Section 5.2.3 addresses the use of the metadata.


                                                        80


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

2676   5.2.2.6. Association With Other Related Key Information
2677   An association may need to be maintained between protected key information and the keying
2678   material that is used to protect that key information. In addition, keys may require association
2679   with other keying material (see Sec. 5.2.1.6).
2680   Storing the key information together or providing some linkage or pointer between the
2681   information helps satisfy the association requirement. Often, the linkage between a key and
2682   the information it protects is accomplished by providing an identifier for a key, storing the
2683   identifier in the key’s metadata, and storing the key’s identifier with the data protected by
2684   the key. The association shall be maintained for as long as the protected data needs to be
2685   processed.

2686   5.2.2.7. Keying Material Storage Media and Mechanisms
2687   Both cryptographic algorithms and the associated keying material may be stored in various
2688   places, including within the software or hardware running the algorithm, in a [FIPS 140-3]-
2689   validated cryptographic module, in an HSM, in a Trusted Platform Module (TPM), in random
2690   access memory (RAM), on external digital media, and on paper.

2691   5.2.2.7.1 Cryptographic Modules
2692   A cryptographic module is a set of hardware, software, and/or firmware that implements
2693   security functions and is contained within a cryptographic boundary. A module can be
2694   implemented in hardware, software, firmware, or a hybrid. The use of a [FIPS 140-3]-
2695   validated cryptographic module is strongly recommended. FIPS 140 references the
2696   requirements specified in [ISO/IEC 19790]. Four implementation security levels are specified
2697   that increase in terms of the amount of protection provided:
2698       1. Security Level 1 provides the lowest level of security. Software or firmware modules
2699          may operate in a non-modifiable, limited, or modifiable operating environment, such
2700          as a general-purpose computing system. If a module is implemented in hardware, no
2701          specific physical security mechanisms are required beyond the basic requirement for
2702          production-grade components.
2703            A Security Level 1 cryptographic module often relies on the physical security, network
2704            security, and administrative procedures of the system in which it resides. For example,
2705            the protection of keying material may need to be handled by the host system rather
2706            than the cryptographic module.
2707            The physical environment in which the host system and cryptographic module reside
2708            may play a major role in protecting keying material (e.g., the accessibility to the
2709            system by unauthorized parties) as well as any security procedures for accessing the
2710            host system.
2711       2. Security Level 2 adds a requirement for tamper evidence for the cryptographic
2712          module or the device in which it resides. Tamper-evident seals or pick-resistant locks



                                                        81


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                         Recommendation for Key Management
       December 2025                                                                                             Part 1 — General

2713             are used to protect against unauthorized physical access. Security Level 2 also requires
2714             role-based authentication in which a cryptographic module authenticates the
2715             authorization of an operator to assume a specific role and perform a corresponding
2716             set of services. Software cryptographic modules may be executed in a modifiable
2717             environment (e.g., general-purpose computer) that implements role-based access
2718             controls or, at a minimum, discretionary access control with a robust mechanism for
2719             defining groups and assigning restrictive permissions.
2720             In addition to Security Level 1 controls, a Security Level 2 cryptographic module relies
2721             on role-based operator authentication and tamper evidence to protect the keying
2722             material stored within the module or device.
2723             The security of the keying material also depends on the alertness and trustworthiness
2724             of the system operators, the physical security of the environment, and the security
2725             procedures for accessing the cryptographic module.
2726        3. Security Level 3 adds physical security protection that is intended to have a high
2727           probability of detecting and responding to attempts at physical access, unauthorized
2728           use, or modification of the cryptographic module. Security Level 3 also:
2729                   o Requires identity-based authentication mechanisms to verify that an operator
2730                     is authorized to assume a specific role and perform a corresponding set of
2731                     services;
2732                   o Requires the entry or output of security-related information (e.g., secret and
2733                     private cryptographic keys) using encryption, a trusted channel, or split-
2734                     knowledge procedures; and
2735                   o Protects a cryptographic module against a security compromise due to
2736                     environmental conditions or fluctuations outside of the module’s normal
2737                     operating ranges for voltage and temperature.
2738             In addition to Security Level 2 controls, a Security Level 3 cryptographic hardware
2739             module relies on identity-based operator authentication, tamper response, and the
2740             detection of environmental fluctuations to protect the keying material stored within
2741             the module. Security Level 3 modules also protect the confidentiality and integrity of
2742             secret keying material that is entered into or output from the module.
2743             Only authorized individuals have access to its content, and the confidentiality of its
2744             input and output is protected.
2745        4. Security Level 4 adds requirements for responding to all unauthorized attempts at
2746           physical access. Penetration of the cryptographic module enclosure from any
2747           direction has a very high probability of being detected, resulting in the immediate
2748           zeroization of all plaintext security parameters.34 Security Level 4 introduces a multi-



       34 The disclosure or modification of security-related information (e.g., secret and private cryptographic keys, authentication data) can

       compromise the security of a cryptographic module.



                                                                         82


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

2749            factor authentication requirement for operator authentication. At minimum, this
2750            requires two of the following three attributes:
2751                o Something known, such as a secret password
2752                o Something possessed, such as a physical key or token
2753                o A physical property, such as a biometric
2754            Security Level 4 includes special environmental protection features that are designed
2755            to detect voltage and temperature boundaries and zeroize sensitive information to
2756            provide a reasonable assurance that the module will not be affected when outside of
2757            the normal operating range in a manner that can compromise the security of the
2758            module.
2759            In addition to Security Level 3 controls, a Security Level 4 cryptographic hardware
2760            module relies on multi-factor authentication and enhanced physical mechanisms to
2761            protect the keying material stored within the module. A Security Level 4 cryptographic
2762            module is designed to destroy sensitive information (e.g., keying material) if an
2763            unauthorized attempt is made to access the module.

2764   5.2.2.7.2 Hardware Security Module (HSM)
2765   HSMs are special-purpose hardware devices that securely manage cryptographic keys and
2766   perform cryptographic operations using those keys. HSMs are built with tamper-evident and
2767   tamper-resistant cases and have detection mechanisms that zeroize sensitive data if a breach
2768   is attempted using sensors that detect voltage, temperature, and other environmental
2769   changes that might indicate tampering. HSMs securely manage a key’s life cycle, including its
2770   generation, storage, distribution, revocation, and destruction. Role-based access control is
2771   used to ensure that only authorized personnel can perform sensitive operations.
2772   HSMs are used within the public-key infrastructure (PKI) and digital signature systems, serve
2773   as secure digital wallets, facilitate transaction signing, are used for secure code signing, and
2774   provide security control over sensitive data in the cloud. Current HSMs that manage
2775   quantum-vulnerable cryptography will be or are being updated to support new quantum-
2776   resistant algorithms.
2777   An HSM certified for [FIPS 140-3] cryptographic module compliance provides assurance at
2778   the appropriate security level that the HSM can effectively protect sensitive data and securely
2779   perform cryptographic functions securely. In addition, an HSM with a high Evaluation
2780   Assurance Level (EAL) certification under the Common Criteria standard has demonstrated
2781   the ability to meet additional stringent security requirements.
2782   To protect keying material appropriately, an HSM needs to support the range of
2783   cryptographic algorithms needed by the applications it supports. With the advent of quantum
2784   computing, support for approved quantum-resistant algorithms is recommended. Most of
2785   the approved algorithms require random number inputs, so an approved RBG should be
2786   included in the HSM. Methods for secure back up, archiving, and the recovery of keys should



                                                        83


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

2787   also be provided as well as detailed role-based access control policies and cryptographic
2788   agility to allow for easy changes to cryptographic components.
2789   If using an HSM in conjunction with cloud services, an HSM that supports bring-your-own-key
2790   (BYOK), double key encryption (DKE), and hold-your-own-key (HYOK) integrations is
2791   recommended to provide the flexibility of leveraging cloud services while retaining the ability
2792   to both own and control encryption keys and/or reduce the risk of unauthorized data access
2793   or data loss. For each of these features, the keying material is generated and
2794   cryptographically protected in an HSM owned by a customer of the cloud service provider.
2795   When using the BYOK feature, the customer sends the keying material to the cloud for
2796   storage, and the keying material is cryptographically protected during its transit and in
2797   storage (e.g., by the customer’s key-wrapping key). When DKE is used, the customer sends
2798   the keying material to the cloud provider, and the keying material is encrypted a second time
2799   by the cloud provider before storing the doubly encrypted keying material. Both keys are
2800   required to decrypt and subsequently use the original plaintext keying material. When HYOK
2801   is used, the customer retains the cryptographically protected keying material but sends data
2802   protected by that keying material to the cloud for storage.
2803   Although keying material is protected while remaining within the HSM, it must be
2804   cryptographically protected at an appropriate security strength when leaving the HSM.

2805   5.2.2.7.3 Trusted Platform Module (TPM)
2806   A TPM was originally specified as a dedicated microprocessor that was designed to secure
2807   hardware using integrated cryptographic keys. A TPM provides a random number generator
2808   and the ability to generate cryptographic keys for a limited number of uses, including
2809   verification that the hardware and software within a device have not been changed,
2810   generating and encrypting cryptographic keys that can only be decrypted by the TPM, disk
2811   encryption to protect a computer’s storage devices, and digital rights management. These
2812   chips store the keys and perform cryptographic operations that depend on the keys. At no
2813   time does sensitive, plaintext keying material leave the TPM.
2814   The latest TPM version (TPM 2.0) specifies five types of TPM implementations:
2815       1. Discrete TPMs are dedicated chips that implement TPM functionality and provide
2816          some level of tamper resistance. Discrete TPMs can protect the confidentiality and
2817          integrity of the keying material within it. Depending on the quality of the tamper-
2818          resistance feature, this protection may equate to a Security Level 2 or 3 cryptographic
2819          module (see Sec. 5.2.2.7.1).
2820       2. Integrated TPMs are only part of a chip and may not provide tamper resistance.
2821          Integrated TPMs within chips that can provide tamper detection can protect the
2822          confidentiality and integrity of the keying material within them. Otherwise, the
2823          protection, if available, would need to be provided by external means. This might
2824          equate to a Security Level 1 cryptographic module.
2825       3. Firmware TPMs are firmware-based implementations that are run in a CPU’s trusted
2826          execution environment, which is a secure area of a computer’s main processor.


                                                        84


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                         Recommendation for Key Management
       December 2025                                                                                             Part 1 — General

2827          4. Virtual TPMs use hypervisors in isolated execution environments that are hidden from
2828             the software running inside virtual machines to secure their code from the software
2829             in the virtual machines.
2830          5. Software TPMs are emulator TPMs but run with no more protection than a regular
2831             program gets within an operating system.
2832   TPM 2.0 includes several classical cryptographic algorithms: SHA-256, RSA, ECC with curve P-
2833   256, HMAC, and AES-128. Additional algorithms are also defined but may be optional.
2834   Changing algorithms is accomplished by replacing the TPM implementation.
2835   TPMs are commonly available on many computer systems. To utilize a TPM, a software library
2836   is used to communicate with the TPM.
2837   The amount of protection provided for keying material depends on the TPM being used.
2838   However, in all cases, keying material must be cryptographically protected at an appropriate
2839   security strength when leaving the TPM.

2840   5.2.2.7.4              Random-Access Memory (RAM)
2841   Random-access memory (RAM) is a form of computer memory that can be read and changed
2842   in any order and is typically used to store working data and machine code. Data in RAM can
2843   be read or written in almost the same amount of time, irrespective of the physical location of
2844   the data inside the memory. However, RAM is normally volatile memory (i.e., stored
2845   information is lost if power is removed). Non-volatile RAM has also been developed, and
2846   other types of non-volatile memories allow random access for read operations but either do
2847   not allow write operations or have other kinds of limitations.
2848   Keying material stored in RAM needs cryptographic protection applied by an appropriate
2849   cryptographic module. If the memory is volatile and the data is to be preserved beyond power
2850   loss, it needs to be backed up or archived (e.g., using external media; see Sec. 5.2.2.7.5).

2851   5.2.2.7.5 External Digital Media
2852   External media may be used to store keying material, such as:
2853          •     A flash drive (also known as a thumb drive) is a data storage device that includes flash
2854                memory 35 with an integrated USB interface. A typical USB drive is removable,
2855                rewritable, and small. Some allow up to 100,000 write/erase cycles, depending on the
2856                exact type of memory chip used, and are thought to physically last between 10 and
2857                100 years under normal circumstances. Common uses of USB flash drives include
2858                storage, supplementary back-ups, and transferring computer files and other data.
2859                However, data loss from a bit leaking due to a prolonged lack of electrical power and
2860                the possibility of spontaneous controller failure due to poor manufacturing could
2861                make it unsuitable for long-term archiving of data.


       35
            Flash memory is an electronic non-volatile computer memory storage medium that can be electrically erased and reprogrammed.



                                                                         85


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                  Recommendation for Key Management
       December 2025                                                                      Part 1 — General

2862            Keying material stored in flash memory needs cryptographic protection at an
2863            appropriate security strength that is applied by a cryptographic module. Flash drives
2864            should be stored in physical safes to ensure the continued availability of the data
2865            stored on them. Cryptographically protected long-term keying material and data
2866            should be backed up or archived using more reliable media.
2867       •    A hard drive is a data storage device that stores and retrieves digital data using
2868            magnetic storage. Data is accessed in a random-access manner, meaning that
2869            individual blocks of data can be stored and retrieved in any order. Hard drives have
2870            non-volatile storage (i.e., stored data is retained when the powered is turned off). The
2871            primary characteristics of a hard drive are its large capacity and performance (i.e., the
2872            time needed to access or write data).
2873            Keying material stored on a hard drive needs cryptographic protection applied by a
2874            cryptographic module at an appropriate security strength.
2875       •    A smart card is a plastic or metal card that contains either a microprocessor and a
2876            memory chip or only a memory chip with non-programmable logic. The
2877            microprocessor on the card can add, delete, and otherwise manipulate information
2878            on the card, while a memory-chip card (e.g., pre-paid phone cards) can only perform
2879            a predefined operation.
2880            There are currently three categories of smart cards:
2881                o    Integrated circuit (IC) microprocessor cards (also known as “chip cards”) offer
2882                     greater memory storage and data security than a traditional magnetic stripe
2883                     card. Chip cards also can process data on the card. These cards are used for a
2884                     variety of applications, especially those that have cryptography built in, which
2885                     requires the manipulation of large numbers.
2886                     Keying material stored on these cards requires cryptographic protection that
2887                     is provided at an appropriate security strength by a cryptographic module
2888                     either on or off the card.
2889                o    IC memory cards have no processor on the card with which to manipulate the
2890                     data stored on it. They depend on a computer for processing and are suitable
2891                     for performing fixed operations (e.g., key storage).
2892                     Keying material stored on these cards requires cryptographic protection that
2893                     is provided at an appropriate security strength by an off-card cryptographic
2894                     module.
2895                o    Optical memory cards can store data that cannot be changed or removed once
2896                     written. Currently, these cards have no processor in them.
2897                     Keying material stored on these cards requires cryptographic protection that
2898                     is provided at an appropriate security strength by an off-card cryptographic
2899                     module.




                                                        86


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
       December 2025                                                                     Part 1 — General

2900            Smart cards have a limited lifetime. Cryptographically protected long-term keying
2901            material and data must be backed up or archived on more reliable, long-term media.
2902       •    Keying material may be printed on or punched into paper or other physical media.
2903            The keying material could be encrypted, split into multiple key components, or
2904            handled using dual-control procedures. Printed or punched keying material can be in
2905            plaintext form, although this is not recommended. When not in use, printed or
2906            punched keying material should be stored in a physical safe.
2907            The keying material should be backed up or archived for as long as it is needed to
2908            remove or verify any cryptographic protection that was applied to the information.

2909   5.2.3. Metadata for Keys
2910   Metadata is used to provide information about a key, including its attributes and intended
2911   use. Different applications may require different metadata elements for the same key type,
2912   and different metadata elements may be required for different key types. Implementers are
2913   responsible for selecting suitable metadata elements for keys. When metadata is used, the
2914   metadata should accompany a key (i.e., the metadata is typically stored or transmitted with
2915   a key). However, depending on an application and implementation, some metadata may be
2916   explicitly known (e.g., all information has the same sensitivity, or all keys are used by a single
2917   application).
2918   Some examples of metadata elements include:
2919       •    The seed used to (re)generate a key or key pair (if appropriate)
2920       •    A key identifier
2921       •    The algorithm to be used with the key
2922       •    Information that identifies associated keys (e.g., the association between a public and
2923            private key)
2924       •    The identity of the key’s owner or the identifiers of the sharing entities
2925       •    If the owner is a non-human entity, the identity of sponsors or representatives for the
2926            owner
2927       •    If the owner is a device or process, the location of the device or process
2928       •    The key’s cryptoperiod (e.g., the start and end dates for using the key to protect or
2929            process information)
2930       •    The key type (e.g., signing private key, encryption key or decapsulation key)
2931       •    The source of the keying material (i.e., the entity that provided the key)
2932       •    The application with which the key is to be used (e.g., purchasing or email)
2933       •    The sensitivity of the information protected by the key
2934       •    A counter, which is used to detect the playback of a previously transmitted key


                                                        87


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)              Recommendation for Key Management
       December 2025                                                                  Part 1 — General

2935       •    The current key state (i.e., pre-activation, active, suspended, compromised, or
2936            destroyed)
2937       •    The key’s status and/or history (i.e., distributed, suspended, or revoked with the
2938            revocation reason)
2939       •    The identity of the key-wrapping key or decapsulation key used to wrap/decapsulate
2940            the key and the algorithm used for wrapping or decapsulation
2941       •    The integrity-protection mechanism used (e.g., the key and algorithm used to provide
2942            cryptographic protection for the key information and the protection code)
2943       •    Other information (e.g., the length of the key, any protection requirements, who has
2944            access rights to the key, or additional conditions for use)
2945   [SP 800-152] provides additional information about the use of metadata, including guidelines
2946   for protecting its integrity and association with the related key.
2947




                                                        88


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                         Recommendation for Key Management
       December 2025                                                                             Part 1 — General

2948   6. Key States and Transitions
2949   A key may pass through several states between its generation and destruction. Figure 3
2950   depicts an example of the key states that a key could assume and the transitions among them.




2951
2952                                       Fig. 3. Key state and transition example

2953   A key may be used differently, depending on its state in the key’s life cycle. Key states are
2954   defined from a system point of view as opposed to the point of view of a single cryptographic
2955   module. The following sections discuss the states that an operational or backed-up key may
2956   assume as well as transitions to other states, as shown in Fig. 3. Additional states may be
2957   applicable for some systems (e.g., deactivated and destroyed compromised states, which
2958   were depicted in the example provided in previous versions of this recommendation), and
2959   some of the identified states may not be needed for a system (e.g., a decision could be made
2960   that the suspended state will not be used).
2961   Transitioning between states shall be recorded. Suitable places for such recordings are audit
2962   logs and the key’s metadata (see Sec. 5.2.3). [SP 800-152] discusses the logging of these
2963   events, including the minimum information that shall be recorded for each event.


                                                             89


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                  Recommendation for Key Management
       December 2025                                                                      Part 1 — General

2964   The following sections discuss the example states and transitions provided in Fig. 3.

2965   6.1. Pre-Activation State
2966   The key has been generated but has not been authorized for use. Keys to be inventoried (e.g.,
2967   long-term keys) shall be inventoried upon generation (see Sec. 8.2).
2968   In this state, the key may only be used to perform proof-of-possession (Sec. 7.1.5.1.1.2) or
2969   key confirmation (see [SP 800-175B]). Otherwise, a key shall not be used to apply
2970   cryptographic protection to information (e.g., encrypt or sign information to be transmitted
2971   or stored) or to process cryptographically protected information (e.g., decrypt ciphertext or
2972   verify a digital signature) while in this state.
2973       •    State Transition 1 (enter the pre-activation state): A key enters the pre-activation
2974            state immediately upon generation.
2975            Information about the generation of the key shall be recorded.
2976       •    State Transition 2 (pre-activation state to the destroyed state): If a key is in the pre-
2977            activation state and will not be needed in the future, the key shall transition directly
2978            from the pre-activation state to the destroyed state (Sec. 6.5). In the case of
2979            asymmetric keys, both keys of the key pair shall transition to the destroyed state.
2980       •    State Transition 3 (pre-activation state to the compromised state): When a key is in
2981            the pre-activation state, and the integrity of the key or the confidentiality of a key
2982            requiring confidentiality protection becomes suspect, then the key shall transition
2983            from the pre-activation state to the compromised state (Sec. 6.4). In the case of
2984            asymmetric keys, both keys of the key pair shall transition to the compromised state.
2985            If the key is known by multiple entities, a revocation notice shall be generated.
2986       •    State Transition 4 (pre-activation state to active state): Keys shall transition from the
2987            pre-activation state to the active state (Sec. 6.2) when the key becomes available for
2988            operational use. This transition may occur upon reaching an activation date or
2989            because of an external event. If keys are generated for immediate use, the transition
2990            occurs immediately after entering the pre-activation state.
2991            For asymmetric keys associated with a certificate, both keys of the key pair become
2992            active upon the notBefore date (if used) in the first certificate issued for the public key
2993            of the key pair.
2994            This transition marks the beginning of the cryptoperiod of a symmetric key or both
2995            keys of an asymmetric key pair (see Sec. 4.3).

2996   6.2. Active State
2997   In the active state, the key may be used to cryptographically protect information (e.g., encrypt
2998   plaintext or generate a digital signature), cryptographically process previously protected
2999   information (e.g., decrypt ciphertext or verify a digital signature), or both. When a key is


                                                        90


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                           Recommendation for Key Management
       December 2025                                                                               Part 1 — General

3000   active, it may be designated for protection only, processing only, or both protection and
3001   processing, depending on its type. For example, asymmetric private signature keys, public
3002   key-transport keys, and public static/ephemeral encapsulation keys are implicitly designated
3003   to only apply protection. Public signature-verification keys, private key-transport keys, and
3004   private static/ephemeral decapsulation keys are designated for processing only. A symmetric
3005   data-encryption key may be used to encrypt data during its originator-usage period and
3006   decrypt the encrypted data during its recipient-usage period (see Sec. 4.3.5). Keys that are
3007   disallowed shall be used for processing only (see [SP 800-131A]).
3008   The use of each key in the active state should be recorded (see [SP 800-152]).
3009       •    State Transition 5 (active state to the destroyed state): Keys transition from the active
3010            state to the destroyed state if they are no longer needed or if the end of their
3011            cryptoperiod is reached, as shown in Table 9.
3012                           Table 9. Transition from the active state to the destroyed state

                       Key Type                                Transition 5: Active to Destroyed State

                                                          Digital Signatures
            1. Private signature key          At the end of the cryptoperiod (e.g., when the notAfter date is reached
                                              on the last certificate issued for the public key)
            2. Public signature-              When no longer needed
               verification key
                                                           Authentication
            3. Symmetric authentication       At the end of the recipient-usage period or when no longer needed
               key
            4. Private authentication key     At the end of the cryptoperiod (e.g., when the notAfter date is reached
                                              on the last certificate issued for the public key)
            5. Public authentication key      When no longer needed
                                                        Random Bit Generation
            6. Symmetric RBG keys              When replaced by a new key or when the RBG will no longer be used
                                                           Key Derivation
            7. Symmetric key derivation       At the end of the originator-usage period or when no longer needed
               key
                                                  Key Establishment (automated)
            8. Symmetric key-                 At the end of the recipient-usage period or when no longer needed
               wrapping/unwrapping key
            9. Public key transport key       At the end of the cryptoperiod (e.g., when the notAfter date is reached
                                              on the last certificate issued for the public key)
            10. Private key transport key     When no longer needed
            11. Symmetric key-agreement
                                              At the end of the recipient-usage period or when no longer needed
                key




                                                               91


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                             Recommendation for Key Management
       December 2025                                                                                 Part 1 — General

                       Key Type                                Transition 5: Active to Destroyed State

            12. Public static key-             At the end of the cryptoperiod (e.g., when the notAfter date is reached
                agreement key                  on the last certificate issued for the public key)
            13. Private static key-            Should usually transition at the end of the public key’s cryptoperiod
                agreement key                  (see [SP 800-131A] for exceptions)
            14. Public ephemeral key-
                agreement key
                                               Immediately after use
            15. Private ephemeral key-
                agreement key
            16. Public static                  At the end of the cryptoperiod (e.g., when the notAfter date is reached
                encapsulation key              on the last certificate issued for the public key)
            17. Private static
                                               When no longer needed
                decapsulation key
            18. Public ephemeral
                encapsulation key
                                               Immediately after use
            19. Private ephemeral
                decapsulation key
                                                     Data Encryption/Decryption
            20. Symmetric data
                encryption/decryption
                key of data in transit
                                               At the end of the recipient-usage period or when no longer needed
            21. Symmetric data
                encryption/decryption
                key of data at rest
                                      Key Storage (for operational, backup, and archive storage)
            22. Symmetric key-
                wrapping/unwrapping            At the end of the recipient-usage period or when no longer needed
                key
            23. Public key-wrapping key        At the end of its cryptoperiod
            24.Private key-unwrapping
                                               When no longer needed
               key
            25. Public encapsulation key       At the end of its cryptoperiod
            26.Private decapsulation key       When no longer needed
                                                            Authorization
            27. Symmetric authorization
                                               At the end of the originator-usage period
                key
            28. Private authorization key      At the end of its cryptoperiod (e.g., when the notAfter date is reached
                                               on the last certificate issued for the corresponding public key)
            29. Public authorization key

3013       •    State Transition 6 (active state to the compromised state): A symmetric key or
3014            asymmetric key pair shall transition from the active state to the compromised state


                                                                92


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                           Recommendation for Key Management
       December 2025                                                                                               Part 1 — General

3015              (see Sec. 6.4) when the confidentiality or integrity of the symmetric key or the
3016              confidentiality of an asymmetric private key becomes suspect. In this case, the key or
3017              key pair shall be revoked. In the case of asymmetric key pairs, the compromise
3018              pertains explicitly to the private key of the key pair, but both keys shall transition to
3019              the compromised state at the same time. For example, when a private signature key,
3020              private key-transport key, or private static/ephemeral decapsulation key is either
3021              compromised or suspected of being compromised, the corresponding public key also
3022              needs to transition to the compromised state.
3023              If the key is known by multiple entities, a revocation notice shall be generated.
3024        •     State Transition 7 (active state to the suspended state): When a suspended state is
3025              used by an application (see Sec. 6.3) and the key or key pair is not to be used for a
3026              period of time (i.e., the period of suspension), a symmetric key or both keys of a key
3027              pair shall transition from the active state to the suspended state. For example, a
3028              private signature key may be suspended because the entity associated with the key is
3029              on a leave of absence or there is suspicion that the key may have been compromised.
3030              In the latter case, the suspension will allow for an investigation of the key’s status
3031              before initiating costly revocation and replacement processes.
3032              Symmetric RBG keys shall transition to the compromised state and be replaced rather
3033              than remaining in the suspended state.
3034              If the key or key pair is known by multiple entities, a notification indicating the
3035              suspension and reason shall be generated.

3036   6.3. Suspended State
3037   The use of a non-ephemeral key or key pair may be suspended for several reasons.36 One
3038   reason might be a possible key compromise, in which case the suspension might be issued to
3039   allow time to investigate the situation. Another reason might be that the entity that owns a
3040   digital signature key pair is not available (e.g., the entity is on an extended leave of absence).
3041   Signatures purportedly signed during the suspension time would be invalid. Depending on
3042   the reason for the suspension, a suspended key or key pair may be restored to an active or
3043   destroyed state or may transition to the compromised state.
3044   A suspended key shall not be used to apply cryptographic protection (e.g., encrypt plaintext
3045   or generate a digital signature). However, depending on the reason for the suspension, a
3046   suspended key could be used to process information for which cryptographic protection was
3047   known to be applied before the suspension period. This includes the use of a public-
3048   verification key to verify a digital signature, a symmetric data-encryption key to decrypt
3049   encrypted information, a symmetric key-wrapping key to unwrap keying material, and a
3050   private decapsulation key to decapsulate an encapsulated key. If the reason for the
3051   suspension is a suspected compromise, it may not be prudent to verify any signatures using
3052   the public key until and unless the key pair is subsequently reactivated (i.e., the key was not

       36 In the case of asymmetric key pairs, both the public and private keys shall be suspended at the same time.




                                                                           93


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                            Recommendation for Key Management
       December 2025                                                                                Part 1 — General

3053   compromised), even if the risk is acceptable. However, if the reason is that the owner is on a
3054   leave of absence, verifying signatures known to be generated before the beginning of the
3055   suspension period may be warranted.
3056   Information for which protection is known to have been applied during the suspension period
3057   shall not be processed.
3058        •    State Transition 8 (suspended state to the active state): A key or key pair in the
3059             suspended state shall transition to the active state when the reason for the
3060             suspension no longer exists and the end of the cryptoperiod has not been reached. In
3061             the case of symmetric keys, the transition needs to be made before the end of the
3062             key’s recipient-usage period.
3063        •    State Transition 9 (suspended state to the destroyed state): Keys may transition from
3064             the suspended state to the destroyed state (Sec. 6.5) if no compromise has been
3065             determined and the key is either no longer needed or the end of the cryptoperiod has
3066             been reached. See Table 9 in Sec. 6.2 for transitions to the destroyed state for each
3067             key type.
3068        •    State Transition 10 (suspended state to the compromised state): A key or key pair in
3069             the suspended state shall transition to the compromised state when the integrity of
3070             the key or the confidentiality of a key requiring confidentiality protection becomes
3071             suspect or is confirmed. In this case, the key or key pair shall be revoked.
3072             In the case of asymmetric key pairs, both the public and private keys shall transition
3073             at the same time.
3074             If the key is known by multiple entities, a revocation notice shall be generated.

3075   6.4. Compromised State
3076   Generally, keys are compromised when they are provided to or determined by an
3077   unauthorized entity. A compromised key 37 shall not be used to apply cryptographic
3078   protection to information. However, a compromised symmetric key or a public key that
3079   corresponds to a compromised private key of a key pair may sometimes be used to process
3080   cryptographically protected information. For example, a signature may be verified to
3081   determine the integrity of signed data if its signature has been physically protected since a
3082   time before the compromise occurred or a reliable timestamp has been included in the signed
3083   data. This processing shall be done only under highly controlled conditions and when the
3084   users of the information are fully aware of the possible consequences.
3085        •    State Transition 11: The following keys shall transition immediately to the destroyed
3086             state when a compromise is suspected:
3087                     1.         Private signature key
3088                     4.         Private authentication key

       37 Keys retrieved from an archive may be in the compromised state.




                                                                            94


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                   Recommendation for Key Management
       December 2025                                                                       Part 1 — General

3089                   6.       Symmetric RBG key
3090                   7.       Symmetric key-derivation
3091                   9.       Public key-transport key
3092                   12-15. Private and public key-agreement keys (both static and ephemeral)
3093                   16, 18. Public encapsulation keys for key storage (both static and ephemeral)
3094                   22.      Symmetric key-wrapping/unwrapping key
3095                   23.      Public key wrapping key for key storage
3096                   25.      Public encapsulation key for key storage
3097                   27 .     Symmetric authorization key
3098                   28, 29. Public and private authorization keys
3099            The following keys may continue to be used to process protected information when
3100            needed unless accepting the processed information carries too much risk, at which
3101            time they shall transition to the destroyed state:
3102                   2.       Public signature-verification key (to verify signatures generated using the
3103                            corresponding private signature key)
3104                   3.       Symmetric authentication key (to verify a MAC generated using this key)
3105                   5.       Public authentication key (to verify signatures generated using the
3106                            corresponding private authentication key)
3107                   8.       Symmetric key-wrapping/unwrapping key (to unwrap keys that were
3108                            wrapped using this symmetric key-wrapping key)
3109                   10.      Private key-transport key (to decrypt keys that were encrypted using the
3110                            corresponding public key-transport key)
3111                   11.      Symmetric key-agreement key (to determine an agreed-upon key)
3112                   17, 19. Private decapsulation key (to decapsulate a key that was encapsulated
3113                           for key establishment, both static and ephemeral)
3114                   20, 21. Symmetric data encryption/decryption key (to decrypt data that was
3115                           encrypted using this key)
3116                   24.      Private key unwrapping key for storage (to unwrap stored keys)
3117                   26.      Private decapsulation key (to decapsulate stored keys)
3118   Keys should transition to the destroyed state when no longer needed.

3119   6.5. Destroyed State
3120   Transitioning to this state results in the destruction of a key. Even though the key no longer
3121   exists when in this state, certain metadata (e.g., the owner’s identity, key state transition


                                                           95


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)            Recommendation for Key Management
       December 2025                                                                Part 1 — General

3122   history, key name, type, and/or cryptoperiod) may be retained for audit purposes (see Sec.
3123   7.4).
3124   A compromise of a key could be determined after the key has already been destroyed. In this
3125   case, the event shall be recorded (see [SP 800-152]).
3126




                                                        96


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

3127   7. Key-Management Phases and Functions
3128   The cryptographic key-management lifecycle can be divided into four phases. During each
3129   phase, the keys are in certain specific key states as discussed in Section 6. In addition, within
3130   each phase, certain key-management functions are typically performed. These functions are
3131   necessary for the management of the keys and their associated metadata.
3132   Key-management information is called metadata and may include the identity of a person or
3133   system associated with that key or the types of information that person is authorized to
3134   access. Metadata is used by applications to select the appropriate cryptographic keys for a
3135   particular service. While the metadata does not appear in cryptographic algorithms, it is
3136   crucial to the implementation of applications and application protocols. See Sec. 5.2.3 for
3137   additional discussion about metadata.
3138   During the phases of the cryptographic key-management life cycle, the keys are in specific
3139   key states (see Sec. 6), and certain key-management functions are typically performed. These
3140   functions are necessary for the management of the keys and their associated metadata.
3141   The four phases of key management are:
3142       1. Pre-operational phase: The keying material is not yet available for normal
3143          cryptographic operations. Keys may not yet be generated or are in the pre-activation
3144          state. System or enterprise attributes are established during this phase as well.
3145       2. Operational phase: The keying material is available and in normal use. Keys are in the
3146          active or suspended state. Keys in the active state may be designated as protect only,
3147          process only, or allowed for both protection and processing. Keys in the suspended
3148          state can sometimes be used for processing only (see Sec. 6.3).
3149       3. Post-operational phase: The keying material is no longer in normal use, but access to
3150          the keying material is possible, and the keying material may be used for processing
3151          protected information. Keys are in the compromised state or have been temporarily
3152          reactivated from an archive (see Sec. 7.3.1).
3153       4. Destroyed phase: Keys are in the destroyed state and no longer available. Records of
3154          their existence may or may not have been destroyed, but the key’s metadata (e.g.,
3155          key name, type, cryptoperiod, usage period) may be retained (see Sec. 7.4).
3156   A flow diagram for the key-management phases is presented in Fig. 4.




                                                        97


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                         Recommendation for Key Management
       December 2025                                                                             Part 1 — General

                                                            1

                                              2                                   3
                                                        Pre-Operational
                                                             Phase


                                                            4

                                                                                  5
                                                          Operational
                                                            Phase


                                                            6


                                              7
                                                        Post-Operational
                                                             Phase




                                                           Destroyed
                                                             Phase
3157
3158                                              Fig. 4. Key-management phases

3159   There are seven phase transitions, and a key shall not be able to transfer back to any
3160   previous phase:
3161       1. Phase Transition 1: A key is in the pre-operational phase upon generation (pre-
3162          activation state).
3163       2. Phase Transition 2: If keys are produced but never used, they may be destroyed by
3164          transitioning from the pre-operational phase directly to the destroyed phase.
3165       3. Phase Transition 3: When a key in the pre-operational phase is compromised, it
3166          transitions to the post-operational phase (compromised state).
3167       4. Phase Transition 4: After the required metadata has been established, keying material
3168          has been generated, and the metadata is associated with the key during the pre-
3169          operational phase, the key is ready to be used by applications and transitions to the
3170          operational phase at the appropriate time.
3171       5. Phase Transition 5: When a key in the operational phase is compromised, it transitions
3172          to the post-operational phase (compromised state).




                                                                98


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                       Recommendation for Key Management
       December 2025                                                                           Part 1 — General

3173       6. Phase Transition 6: When keys are no longer required for normal use (e.g., the end of
3174          the cryptoperiod has been reached and the key is no longer “active”) but access to
3175          those keys needs to be maintained, the key transitions to the post-operational phase.
3176       7. Phase Transition 7: Some applications will require that access be preserved for a
3177          period of time, and then the keying material may be destroyed. When it is clear that
3178          a key in the post-operational phase is no longer needed, it may transition to the
3179          destroyed phase.
3180   The combination of key states and key phases is illustrated in Fig. 5.




3181
3182                                      Fig. 5. Key-management states and phases

3183   The following subsections discuss the functions that are performed in each phase of key
3184   management. References to “systems” refer to systems that handle keys, whether the system
3185   is a client using some protocol or an entire key-management system. A key-management
3186   system may perform many of the functions but not have all identified functions, since some
3187   functions may not be appropriate. In some cases, one or more functions may be combined,
3188   or the functions may be performed in a different order. For example, a system may omit the


                                                            99


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
       December 2025                                                                     Part 1 — General

3189   functions of the post-operational phase if keys are immediately destroyed when they are no
3190   longer used to apply cryptographic protection or are compromised. In this case, keys would
3191   move from the operational phase directly to the destroyed phase.

3192   7.1. Pre-Operational Phase
3193   During the pre-operational phase of key management, keying material is not yet available for
3194   normal cryptographic operations.

3195   7.1.1. Entity Registration Function
3196   During registration into a key-management system, an entity interacts with a registration
3197   authority to become an authorized member of a security domain. In this phase, an entity
3198   identifier may be established to identify the member during future transactions. In particular,
3199   security infrastructures may associate the identification information with the entity’s keys
3200   (see Sec. 7.1.5 and 7.1.6). The entity may supply various information, such as an email address
3201   and the entity’s role, and authorization information may be established by the system
3202   administrator. As with identity information, the infrastructure may associate this information
3203   with the entity’s keys to support secure application-level security services.
3204   Since applications will depend on the identity established during this process, it is crucial that
3205   appropriate procedures for the validation of identity (i.e., identity proofing) be established
3206   and used. Identity proofing is often performed by an organization (e.g., by the organization’s
3207   security office) but may also be performed by a registration authority for the key-
3208   management system. The strength (or weakness) of a security infrastructure will often
3209   depend on the identification process. [FIPS 201-3] and [SP 800-63] address requirements for
3210   establishing identity.
3211   Entity and key registration (see Sec. 7.1.6) may be performed separately or at the same time.
3212   If performed separately, the entity registration process will generally establish a secret value
3213   (e.g., a password or PIN, MAC key) that may be used to authenticate the entity’s identity
3214   during the key-registration step. If entity and key registration are performed at the same
3215   time, the entity establishes an identity and performs key registration in the same process, so
3216   the secret value is not required.

3217   7.1.2. System Initialization Function
3218   System initialization involves setting up or configuring a system for secure operation. For
3219   systems that handle keys, this may include algorithm preferences, the identification of
3220   trusted parties, and the definition of algorithm parameter policies and any trusted
3221   parameters (e.g., recognized certificate policies).




                                                        100


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                   Recommendation for Key Management
       December 2025                                                                       Part 1 — General

3222   7.1.3. Initialization Function
3223   Initialization consists of an entity that initializes its cryptographic application (e.g., installing
3224   and initializing software or hardware). This involves the use or installation of any initial keying
3225   material that may be obtained during entity registration (see Sec. 7.1.4). Examples include
3226   the installation of a key at a CA, trust parameters, policies, trusted parties, and algorithm
3227   preferences.

3228   7.1.4. Keying-Material Installation Function
3229   The security of keying-material installation is crucial to the security of a system, whether the
3230   keying material is generated within the system (e.g., in the system’s cryptographic module)
3231   or generated within another system and subsequently installed in the target system. For this
3232   function, keying material is installed for operational use within an entity’s software,
3233   hardware, system, application, cryptographic module, or device using a variety of techniques.
3234   Keying material is installed during initial setup when new keying material is added to the
3235   existing keying material and when the existing keying material is replaced (e.g., via re-keying
3236   or key derivation; see Sec. 7.2.3 and 7.2.4).
3237   The process for the initial installation of keying material (e.g., by manual entry, using an
3238   electronic key loader, or a vendor during manufacture) shall include the protection of the
3239   keying material during entry into a software, hardware, system, application, device, or
3240   cryptographic module and include any additional procedures that may be required. The
3241   process should account for the requirements of [FIPS 140-3] and its differing requirements
3242   for the different levels of protection.
3243   Many manufacturers provide applications or systems with keying material that is used to test
3244   that the newly installed application/system is functioning properly. This test keying material
3245   shall not be used operationally.

3246   7.1.5. Key-Establishment Function
3247   Key establishment involves the generation and distribution or the agreement of keying
3248   material for communication between entities. All keys shall be generated within a [FIPS 140-
3249   3]-validated cryptographic module or obtained from another source that is approved to
3250   protect national security information. Long-term keys shall be inventoried (see Sec. 8.2).
3251   During the key-establishment process, some of the keying material may be in transit (i.e., the
3252   keying material is being distributed manually or using automated protocols). Other keying
3253   material may be retained locally rather than distributed. In either case, the keying material
3254   shall be protected in accordance with Sec. 5.
3255   An entity may be an individual (human), organization, device, or process. When keying
3256   material is generated by an entity for its own use, one or more of the appropriate protection
3257   mechanisms for stored information in Sec. 5.2.2 shall be used. The “owner” of a key is an
3258   entity that is authorized to use the key. When the owner is not a human (i.e., the owner is an
3259   organization, device, or process), the owner is often assisted by an authorized human


                                                        101


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
       December 2025                                                                     Part 1 — General

3260   representative or sponsor to obtain and manage keying material (e.g., to obtain and install
3261   keys and certificates).
3262   Keying material that is distributed between entities or among an entity and its sub-entities
3263   (e.g., various individuals, devices, or processes within an organization) shall be protected
3264   during distribution using one or more of the appropriate protection mechanisms specified in
3265   Sec. 5.2.1. Any keying material that is not distributed (e.g., the private key of a key pair or
3266   one’s own copy of a symmetric key) or keying material that is received and subsequently
3267   stored shall be protected using one or more of the appropriate protection mechanisms
3268   specified in Sec. 5.2.2.
3269   Sections 7.1.5.1 and 7.1.5.2 discuss the generation and distribution of asymmetric and
3270   symmetric keys, respectively. [SP 800-133] discusses the generation of keying material.

3271   7.1.5.1. Generation and Distribution of Asymmetric Key Pairs
3272   Key pairs shall be generated in accordance with the mathematical specifications of the
3273   appropriate approved FIPS or NIST SP.
3274   A static key pair shall be generated by 1) the entity that owns the key pair (i.e., the entity that
3275   uses the private key in the cryptographic computations) or 2) a trusted facility that distributes
3276   the key pair in accordance with Sec. 7.1.5.1.3, or 3) the owner and facility in a cooperative
3277   process.
3278   In the case of a digital signature key pair (i.e., a public signature-verification key and its
3279   associated private signature key), the intended owner of the key pair should generate the
3280   keying material rather than any other entity generating the keying material for that owner.
3281   This will facilitate support for non-repudiation. When generated by the entity that owns the
3282   key pair, the private signature key shall not be distributed to other entities. However, when
3283   the owner is an organization, it is acceptable to distribute the keying material to the
3284   organization’s sub-entities (e.g., employees or devices). In this case, the organization is the
3285   true owner, and the sub-entities represent the owner.
3286   Ephemeral keys are often used for key establishment instead of or in addition to the use of
3287   static keys (see [SP 800-56A] and [SP 800-227]). Ephemeral key pairs are generated for each
3288   new key-establishment transaction (e.g., unique to each message or session) by the owner.
3289   The generated key pairs shall be protected in accordance with Sec. 5.1.1.

3290   7.1.5.1.1.        Distribution of Public Keys
3291   Static public keys are relatively long-lived and typically used for several executions of an
3292   algorithm. Ephemeral public keys are used only once. The distribution of the static or
3293   ephemeral public key should provide assurance to the receiver of the public key that the true
3294   owner of the key is known (i.e., that the claimed owner is the actual owner). This requirement
3295   may be disregarded if anonymity is acceptable. However, the strength of the overall




                                                        102


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                              Recommendation for Key Management
       December 2025                                                                                                  Part 1 — General

3296   architecture and trust in the validity of the protected data largely depends on assurance of
3297   the public-key owner’s identity.
3298   In addition, the distribution of the public key shall provide assurance to the receiver that:
3299        1. The purpose/usage of the key is known (e.g., for RSA digital signatures, elliptic-curve
3300           key agreement, key encapsulation);
3301        2. Any parameters associated with the public key are known;
3302        3. The public key is valid (e.g., the public key satisfies the required arithmetical
3303           properties); and
3304        4. The owner actually possesses the corresponding private key.

3305   7.1.5.1.1.1.          Distribution of a Trust Anchor’s Public Key in a PKI
3306   The public key of a trusted certification authority (CA) is the foundation for all PKI-based
3307   security services; the trusted CA is considered to be a trust anchor.38 The trust anchor’s public
3308   key is not a secret, but the authenticity of that public key is the crucial assumption for a PKI.
3309   Trust anchor public keys may be obtained through many different mechanisms and provide
3310   different levels of assurance. The types of mechanisms may depend on the role of an entity
3311   in the infrastructure. An entity that only ever acts as a “relying party” (i.e., an entity that may
3312   not have keys registered with the infrastructure) may use different mechanisms than an
3313   entity that possesses keys registered by the infrastructure. Trust-anchor public keys are
3314   frequently distributed as root-CA certificates, which are “self-signed” X.509 certificates that
3315   are signed by the private key corresponding to the public key in the certificate.
3316   Trust-anchor certificates are often embedded in and distributed with an application. For
3317   example, the installation of a new web browser typically includes the installation or
3318   replacement of an entity’s list of trust-anchor certificates. Operating systems are usually
3319   shipped with trust-anchor certificates for a variety of reasons, including the validation of
3320   other certificates. The entity relies on the authenticity of the software distribution
3321   mechanism to ensure that only valid trust-anchor certificates are installed during installation
3322   or replacement.
3323   Other applications sometimes install trust-anchor certificates in web browsers for several
3324   purposes, including the validation of the TLS certificates presented in TLS protocols. Entities
3325   that visit a “secure” website that has a certificate that was not issued by a trust anchor CA
3326   may be given an opportunity to accept that certificate, either for a single session or
3327   permanently.
3328                     Roaming users should be aware that they are implicitly trusting all
3329                     software on the host systems that they use. They should have
3330                     concerns about trust-anchor certificates used by web browsers when
3331                     they use systems in kiosks, libraries, internet cafes, or hotels, as well

       38 While this document refers to a trusted CA as the “trust anchor” and its certificate as the “trust-anchor certificate,” many other documents

       use the term “trust anchor” to refer to both the trusted CA and the CA’s certificate.



                                                                            103


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                  Recommendation for Key Management
       December 2025                                                                      Part 1 — General

3332                 as systems provided by conference organizers to access “secure
3333                 websites.” The user has had no control over the trust-anchor
3334                 certificates installed in the host system, and therefore, the user is
3335                 relying on the host system administrators to have made good,
3336                 sensible decisions about which trust-anchor certificates are allowed.
3337                 Relying parties should be cautious about accepting certificates from
3338                 unknown CAs so that they do not inadvertently add new permanent
3339                 trust-anchor certificates that are not trustworthy. Relying parties
3340                 are not participants in trust-anchor certificate selection when the
3341                 trust-anchor certificates are pre-installed prior to software
3342                 distribution and may have had no part in decisions about which trust-
3343                 anchor certificates are installed thereafter. Users should be aware
3344                 that they are trusting the software distribution mechanism to not
3345                 have resulted in the installation of malicious code. Extending this
3346                 trust to cover trust-anchor certificates for a given application may be
3347                 reasonable and allows the relying party to obtain trust-anchor
3348                 certificates without any additional procedures.
3349   An entity (or an entity’s representative) interacts securely with the infrastructure to register
3350   its keys (e.g., to obtain certificates), and these interactions may be extended to provide trust-
3351   anchor information in the form of a trust-anchor certificate. This allows the establishment of
3352   trust-anchor certificates with approximately the same assurance that the infrastructure has
3353   in the entity’s keys. In the case of a PKI:
3354       1. The initial distribution of a trust-anchor certificate should be performed in
3355          conjunction with the presentation of a requesting entity’s public key to a registration
3356          authority (RA) or CA during the certificate-request process. In general, the trust
3357          anchor’s public key, associated parameters, key use, and assurance of possession are
3358          conveyed as a self-signed X.509 public-key certificate (also called a root-CA
3359          certificate). In this case, the certificate has been digitally signed by the private key that
3360          corresponds to the public key within the certificate. While the parameters and
3361          assurance of possession may be conveyed in the self-signed certificate, the identity
3362          associated with the trust-anchor certificate and other information cannot be verified
3363          from the self-signed certificate itself (see item 2 below).
3364       2. The trusted process used to convey a requesting entity’s public key and assurances to
3365          the RA or CA shall also be used to protect the trust-anchor’s certificate that is
3366          conveyed to the requesting entity. If the requesting entity (or the entity’s
3367          representative) appears in person, the trust-anchor’s certificate may be provided at
3368          that time. If a secret value has been established during entity registration (see Sec.
3369          7.1.1), the trust-anchor’s certificate may be supplied along with the requesting
3370          entity’s certificate.




                                                        104


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                          Recommendation for Key Management
       December 2025                                                                                              Part 1 — General

3371   7.1.5.1.1.2.         Submission to a Registration Authority or Certification Authority
3372   Public keys may be provided to a CA or RA for subsequent certification by a CA. During this
3373   process, the RA or CA shall obtain the assurances listed in Sec. 7.1.5.1.1 and the owner’s
3374   identity from the entity submitting the key (i.e., the owner of the key or an authorized
3375   representative).
3376   In general, the owner of the key is identified in terms of an identifier established during entity
3377   registration (see Sec. 7.1.1). During entity registration, the appropriate uses for the key are
3378   identified along with any required parameters. If anonymous ownership of the public key is
3379   acceptable, the submitter or the registration authority determines a pseudonym to be used
3380   as the identifier. The identifier shall be unique for the naming authority. 39
3381   Proof of possession (POP) is a mechanism that is commonly used by a CA to obtain assurance
3382   of private-key possession during key registration. In this case, the proof shall be provided by
3383   the reputed owner of the key pair. Without assurance of possession, it would be possible for
3384   the CA to bind the public key to the wrong entity.
3385   The (claimed) owner should provide POP by performing operations with the private key that
3386   satisfy the indicated key use. For example:
3387        •    If a key pair is intended for ML-DSA digital signature generation, the CA may provide
3388             information to be signed using the owner’s private key. If the CA can correctly verify
3389             the signature using the corresponding public key, then the owner has established POP.
3390        •    If a key pair is intended for key transport (as specified in [SP 800-56B]) or key
3391             encapsulation (as specified in [SP 800-227]), the CA may provide ciphertext using the
3392             owner’s public key. If the owner can correctly decrypt the ciphertext, then the owner
3393             has established POP.
3394        •    When a key pair is intended to support key establishment (i.e., either key agreement
3395             or key transport), and the key pair was generated as specified in [FIPS 186-5] 40 (see
3396             [SP 800-56A] and [SP 800-56B]), POP may be afforded by using the private key to
3397             digitally sign the certificate request, although this is not the preferred method. The
3398             private key-establishment key (i.e., the private key-agreement or private key-
3399             transport key) shall not be used to perform signature operations after certificate
3400             issuance.
3401   As with entity registration, the strength of the security infrastructure depends on the
3402   methods used for distributing the key to an RA or CA. There are many different methods,
3403   each appropriate for some range of applications. For example:
3404        •    The public key and the information identified in Sec. 7.1.5.1.1 are provided in person
3405             by the public-key owner or an authorized representative of the public-key owner (e.g.,
3406             organization, device, process).

       39
          The naming authority is the entity responsible for the allocation and distribution of domain names, ensuring that the names are unique
       within the domain. A naming authority is often restricted to a particular level of domains, such as .com, .net, or .edu.
       40 SP 800-56A also allows the use of predefined groups for finite-field Diffie-Hellman key agreement. POP cannot be performed using a

       digital signature when these parameters are used.



                                                                         105


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                  Recommendation for Key Management
       December 2025                                                                      Part 1 — General

3407       •    The identity of the public-key owner or an authorized representative of the public-key
3408            owner is established and their authorization verified in person by the RA or CA during
3409            entity registration. At this time, the RA or CA provides unique, unpredictable
3410            information (e.g., authenticator, cryptographic key) to the owner or authorized
3411            representative as a secret value. The information identified in Sec. 7.1.5.1.1 and the
3412            public key are provided to the RA or CA using a communication protocol protected by
3413            the secret value. As specified in Sec. 7.3.4, the secret value should be destroyed by
3414            the key owner or owner’s representative after receiving confirmation that the
3415            certificate has been successfully generated. The RA or CA may maintain this secret
3416            value for auditing purposes, but the RA or CA should not accept further use of the
3417            secret value to prove identity.
3418            When a specific list of public-key owners is preauthorized to register keys, identifiers
3419            may be assigned without the owners being present. In this case, it is critical to protect
3420            the secret values from disclosure, and the procedures shall demonstrate that the
3421            chain of custody was maintained. The lifetime of the secret values should be limited
3422            but shall allow for the public-key owner or the owner’s representative to appear at
3423            the RA or CA, generate the keys, and provide the public key (under the secret value’s
3424            protection) to the RA or CA. Since it may take some time for the public-key owner or
3425            owner’s representative to appear at the RA or CA, a two or three-week lifetime for
3426            the secret value is reasonable.
3427            When public-key owners are not preauthorized, the RA or CA shall determine the
3428            identifier in the presence of the owner or representative. In this case, the time limit
3429            may be much more restrictive if the key pair is generated on-site and the public key is
3430            provided to the CA or RA. In this case, a 24-hour lifetime for the secret value would
3431            be reasonable.
3432       •    The identity of the public-key owner is established at the RA or CA using a previous
3433            determination of the public-key owner’s identity. The continued authorization to
3434            receive a certificate is also verified. Recertification involves “chaining” a new public-
3435            key certificate request to a previously certified digital -signature key pair. For example,
3436            the request for a new public-key certificate is signed by the owner of the new public
3437            key to be certified. The private signature key used to sign the request should
3438            correspond to a public signature-verification key that was certified by the same CA
3439            that will certify the new public key. The request contains the new public key and any
3440            key-related information (e.g., key use and parameters). In addition, the CA shall
3441            obtain assurance of public-key validity and assurance that the owner possesses the
3442            corresponding private key.
3443       •    The public key, key use, parameters, validity assurance information, and assurance of
3444            possession are provided to the RA or CA along with a claimed identity for the owner
3445            and the authorization to receive a certificate. The RA or CA delegates the verification
3446            of the public-key owner’s identity and authorization to another trusted process (e.g.,
3447            an examination of the public-key owner’s identity by the U.S. Postal Service when
3448            delivering registered mail containing the requested certificate). Upon receiving a


                                                        106


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                  Recommendation for Key Management
       December 2025                                                                      Part 1 — General

3449            request for certification, the RA or CA generates and sends unique, unpredictable
3450            information (e.g., an authenticator or cryptographic key) to the requestor using a
3451            trusted process (e.g., registered mail sent via the U.S. Postal Service). The trusted
3452            process assures that the identity of the requestor is verified prior to delivery of the
3453            information provided by the RA or CA. The owner or owner’s representative uses this
3454            information to prove that the trusted process succeeded, and the RA or CA
3455            subsequently delivers the certificate to the owner or the owner’s representative. As
3456            specified in Sec. 7.3.4, the unique, unpredictable information should be destroyed by
3457            the key owner or representative after receiving confirmation that the certificate was
3458            successfully generated. The RA or CA may maintain this information for auditing
3459            purposes but should not accept further use of the unique identifier to prove identity.
3460       •    The public key and Domain Name System (DNS) addresses to be included in the
3461            common name or subject alternative names of the certificate are provided over a
3462            network connection to the RA via a certificate signing request. The RA confirms that
3463            the requestor is authorized to request a certificate for the DNS addresses by
3464            performing a challenge via the DNS record for the addresses, performing a challenge
3465            via Hypertext Transfer Protocol (HTTP) to the DNS addresses, or obtaining some other
3466            form of verification to confirm authorization for the DNS addresses.
3467   In cases that involve an RA, upon receipt of all information from the requesting entity (e.g.,
3468   the owner of the new public key), the RA forwards the relevant information to a CA for
3469   certification. Together, the RA and CA shall perform any validation or other checks required
3470   for the algorithm with which the public key will be used (e.g., public-key validation) prior to
3471   issuing a certificate. The CA should indicate the checks or validations that will be or have been
3472   performed (e.g., in the certificate, certificate policy, or certification practice statement). After
3473   generation, the certificate is distributed manually or using automated protocols to the RA,
3474   the public-key owner or the owner’s representative, or a certificate repository (i.e., a
3475   directory) in accordance with the CA’s certification practice statement.

3476   7.1.5.1.1.3.      General Distribution of Static Public Keys
3477   Static public keys may be distributed to entities other than an RA or CA in several ways, such
3478   as:
3479       1. Manual distribution of the public key itself by the owner of the public key or the
3480          owner’s representative (e.g., in a face-to-face transfer or by a bonded courier): The
3481          mandatory assurances listed in Sec. 7.1.5.1.1 shall be provided to the recipient prior
3482          to operational use of the public key.
3483       2. Manual or automated distribution of a public-key certificate by the public-key owner,
3484          the owner’s representative, the CA, or a certificate repository (i.e., a directory): The
3485          mandatory assurances listed in Sec. 7.1.5.1.1 that are not provided by the CA (e.g.,
3486          public-key validation) shall be provided to or performed by the receiver of the public
3487          key prior to the operational use of the key.



                                                        107


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

3488       3. Automated distribution of a public key (e.g., using a communication protocol with
3489          authentication and content integrity): The mandatory assurances listed in Sec.
3490          7.1.5.1.1 shall be provided to the receiving entity prior to operational use of the public
3491          key.

3492   7.1.5.1.2.        Distribution of Ephemeral Public Keys
3493   When used, ephemeral public keys are distributed as part of a secure key-establishment
3494   protocol. The key-establishment process (i.e., the key-establishment scheme, protocol, key
3495   confirmation, any associated negotiation, and/or local processing) should provide a recipient
3496   with the assurances listed in Sec. 7.1.5.1.1. The recipient of an ephemeral public key shall
3497   obtain assurance of validity of that key, as specified in [SP 800-56A] or [SP 800-227], prior to
3498   using that key for subsequent steps in the key-establishment process.

3499   7.1.5.1.3.        Distribution of Centrally Generated Key Pairs
3500   When a static key pair is centrally generated, the key pair shall be generated within a [FIPS
3501   140-3]-validated cryptographic module or obtained from another source that is approved by
3502   the U.S. Government for protecting national security information for subsequent delivery to
3503   the intended owner of the key pair. A signing key pair generated by a central key-generation
3504   facility for its subscribers will not provide strong support for non-repudiation for those
3505   individual subscribers. Therefore, when non-repudiation is required, the subscribers should
3506   generate their own signing key pairs. However, if the central key-generation facility generates
3507   signing key pairs for its own organization and distributes them to members of the
3508   organization, then support for non-repudiation may be provided at an organizational level
3509   (but not an individual level).
3510   The private key of a key pair generated at a central facility shall only be distributed to the
3511   intended owner of the key pair or provided to the owner’s representative for subsequent
3512   installation. The confidentiality of the centrally generated private key shall be protected, and
3513   the procedures for distribution shall include an authentication of the recipient’s identity and
3514   authorization as established during entity registration (see Sec. 7.1.1).
3515   The key pair may be distributed to the intended owner or owner’s representative using an
3516   appropriate manual method (e.g., courier, mail, or other method specified by the key-
3517   generation facility) or secure automated method (e.g., a secure communication protocol).
3518   The private key shall be distributed in the same manner as a symmetric key (see Sec.
3519   7.1.5.2.2). The distribution of the public key of a key pair is discussed in Sec. 7.1.5.1.1.3.
3520   During the distribution process, each key of the key pair shall be provided with the
3521   appropriate protections for that key (see Sec. 5.1).
3522   Upon receipt of the key pair, the owner shall obtain assurance of the validity of the public
3523   key (see [SP 800-56A], [SP 800-56B], [SP 800-89], and [SP 800-227]). The owner shall obtain
3524   assurance that the public and private keys of the key pair are a consistent pair (e.g., by
3525   checking that a key encrypted under a public key-transport key or a key encapsulated by a
3526   public encapsulation key can be decrypted by the corresponding private key).


                                                        108


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

3527   7.1.5.2. Generation and Distribution of Symmetric Keys
3528   The symmetric keys used for the encryption and decryption of data or other keys and for the
3529   computation of MACs shall be determined by an approved method and shall be provided
3530   with protection that is consistent with Sec. 5.
3531   Symmetric keys shall be:
3532       1. Generated and subsequently distributed (see Sec. 7.1.5.2.1 and 7.1.5.2.2) manually
3533          (see Sec. 7.1.5.2.2.1), using a public key-transport mechanism (see Sec. 7.1.5.2.2.2),
3534          or using a previously distributed or agreed-upon key-wrapping key or KEM (see Sec.
3535          7.1.5.2.2.2);
3536       2. Established using a key-agreement scheme or KEM (i.e., the generation and
3537          distribution are accomplished with one process) (see Sec. 7.1.5.2.3); or
3538       3. Derived from a key-derivation key (see Sec. 7.2.4).

3539   7.1.5.2.1.        Key Generation
3540   Symmetric keys shall be either 1) generated by an approved method (e.g., using an approved
3541   random number generator; see [SP 800-133]) or 2) derived from a key-derivation key (see
3542   Sec. 7.2.4) using an approved key-derivation function (see [SP 800-108]). Symmetric keys may
3543   also be generated using key-agreement or key-encapsulation techniques (see Sec. 7.1.5.2.3),
3544   in which case a separate key-distribution process is not required (e.g., see [SP 800-56A], [SP
3545   800-56B], and [SP 800-227]).
3546   When split-knowledge procedures are used, the key shall only exist outside of a [FIPS 140-3]
3547   cryptographic module as multiple key shares. The key may be created within a cryptographic
3548   module and then split into shares for export from the module, or the key may be created as
3549   separate shares. Each key share shall provide no knowledge of the key value (i.e., each key
3550   share must appear to be generated randomly). If knowledge of k shares is required to
3551   construct the original key, then knowledge of any k−1 key share shall provide no information
3552   about the original key other than, possibly, its length. A suitable combination function is not
3553   provided by simple concatenation (e.g., it is not acceptable to form a 128-bit key by
3554   concatenating two 64-bit key shares).
3555   All keys and key shares shall be generated within a [FIPS 140-3]-validated cryptographic
3556   module or obtained from another source that is approved by the U.S. Government for the
3557   protection of national security information.

3558   7.1.5.2.2.        Key Distribution
3559   Keys generated in accordance with Sec. 7.1.5.2.1 as key-wrapping keys (i.e., key-encrypting
3560   keys), as key-derivation to be used for key derivation, or for the protection of communicated
3561   information are distributed manually (i.e., using manual key-transport procedures) or using
3562   an automated key-transport or KEM protocol.



                                                        109


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

3563   Keys that are only used to protect stored information (i.e., data or keying material) shall not
3564   be distributed except for backup or to other authorized entities that may require access to
3565   the stored information protected by the keys.

3566   7.1.5.2.2.1.      Manual Key Distribution
3567   Keys and key shares that are distributed manually (e.g., by other than an automated key-
3568   transport protocol) shall be protected throughout the distribution process. During manual
3569   distribution, secret keys, private keys, and key shares shall either be wrapped (i.e., encrypted
3570   with integrity protection), encapsulated, or distributed using appropriate physical security
3571   procedures.
3572   If split-knowledge procedures are used for key distribution (i.e., a key is distributed as key
3573   shares; see Sec. 7.1.5.2.1), each key share shall be distributed separately to its intended
3574   recipient.
3575   The process for the manual distribution of secret keys, private keys, and key shares (i.e.,
3576   secret keying material) shall ensure that:
3577       1. The keying material is distributed by an authorized source,
3578       2. Any entity distributing plaintext keying material is trusted by both the entity that
3579          generates the keying material and any entity that receives the keying material,
3580       3. The keying material is protected in accordance with Sec. 5, and
3581       4. The keying material is received by authorized recipients.
3582   When distributed in encrypted form, the key or key share shall either be 1) encapsulated
3583   using an approved KEM and a public encapsulation key owned by the intended recipient, 2)
3584   encrypted using an approved key-wrapping scheme and a key-wrapping key shared with the
3585   intended recipient, or 3) transported using an approved key-transport scheme and a public
3586   key-transport key owned by the intended recipient. The public key or key-wrapping key shall
3587   have been distributed as specified in this recommendation.
3588   Physical security procedures may be used for all forms of manual key distribution. However,
3589   these procedures are particularly critical when the secret keying material is distributed in
3590   plaintext form. In addition to the assurances listed above, accountability and auditing of the
3591   distribution process should be used (see Sect. 8.3 and 8.4).

3592   7.1.5.2.2.2. Automated Key Distribution, Key Transport, Key Encapsulation, and Key
3593                Wrapping
3594   Automated key distribution may be used to distribute secret keys, private keys, and key
3595   shares via a communication channel (e.g., the internet). This requires the distribution or




                                                        110


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

3596   establishment of a key-wrapping key (i.e., a key-encryption key), a public key-transport key,
3597   or public encapsulation key as follows:
3598       •    A key-wrapping key shall be generated and distributed in accordance with Sec.
3599            7.1.5.2.1 and 7.1.5.2.2 or established using a key-establishment scheme, as defined in
3600            Sec. 7.1.5.2.3.
3601       •    A public key-transport key or public static/ephemeral encapsulation key shall be
3602            generated and distributed, as specified in Sec. 7.1.5.1.
3603   Only approved key-wrapping, public key-transport, or key-encapsulation schemes shall be
3604   used. The approved schemes provide assurance that:
3605       •    For symmetric key-wrapping schemes: The key-wrapping key and the distributed
3606            keying material are not disclosed or modified. Approved key-wrapping methods that
3607            provide both confidentiality and integrity protection are provided or referenced in [SP
3608            800-38F].
3609       •    For asymmetric key-transport schemes: The private key-transport key and the
3610            distributed keying material are not disclosed or modified, and correct association
3611            between the private and public key-transport keys is maintained. Approved key-
3612            transport schemes using asymmetric techniques are discussed in [SP 800-56A] and [SP
3613            800-56B].
3614       •    For key-encapsulation schemes: The private decapsulation key and the distributed
3615            ciphertext are not disclosed and/or modified, and correct association between the
3616            private and public keys is maintained. Approved key-encapsulation schemes using
3617            asymmetric techniques are discussed in [SP 800-227].
3618       •    The keying material is protected in accordance with Sec. 5.
3619   In addition, the approved schemes and the associated key-establishment protocol should
3620   provide all of the following assurances:
3621       •    Each entity in the key-distribution process knows the identifier associated with the
3622            other entities;
3623       •    The keying material is correctly associated with the entities involved in the key-
3624            distribution process; and
3625       •    The keying material has been received correctly (e.g., using a key-confirmation
3626            method).

3627   7.1.5.2.3.        Key Agreement and Key Encapsulation
3628   Key establishment using a key-agreement and/or key-encapsulation scheme is used in a
3629   communication environment to establish keying material with information contributed by all
3630   entities in the communication (most commonly, by only two entities). Only approved
3631   schemes shall be used. Approved key-agreement and key-encapsulation schemes using
3632   asymmetric techniques are provided in [SP 800-56A], [SP 800-56B], and [FIPS 203]. Key-


                                                        111


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

3633   agreement schemes use asymmetric key pairs to calculate shared secrets, which are then
3634   used to derive symmetric keys and other keying material (e.g., IVs) without actually
3635   transmitting the keying material. Key encapsulation uses the public key of the intended
3636   recipient and a random value generated by the sending entity to generate ciphertext from
3637   which the recipient can derive the keying material.
3638   A key-agreement or key-encapsulation scheme uses either static or ephemeral asymmetric
3639   key pairs or both. The asymmetric key pairs should be generated and distributed, as discussed
3640   in Sec. 7.1.5.1. Keying material derived using these schemes shall be protected, as specified
3641   in Sec. 5.
3642   A key-agreement or key-encapsulation scheme and its associated key-establishment protocol
3643   should provide the following assurances:
3644       •    The identifiers for entities involved in the key-establishment protocol are correctly
3645            associated with those entities. Assurance for the association of identifiers to entities
3646            may be achieved by the scheme or the protocol in which it is performed. The identifier
3647            may be a “pseudo-identifier” (a pseudonym) rather than, for example, the identifier
3648            that appears on the entity’s birth certificate.
3649            In general, an identifier is associated with each entity involved in the key-
3650            establishment protocol, and each entity must be able to associate all the other entities
3651            with their appropriate identifiers. In special cases, such as the secure distribution of
3652            public information on a website, the association with an identifier may only be
3653            required for a subset of the entities (e.g., only the server).
3654       •    The keys used in the key-agreement or key-encapsulation scheme are correctly
3655            associated with the entities involved in the key-establishment process.
3656       •    The resulting keying material is correct (e.g., determined using a key-confirmation
3657            method).
3658   Keying material that results from the key-agreement or key-encapsulation scheme and its
3659   enabling protocol shall not be used to protect or send information until the three assurances
3660   described above have been obtained.

3661   7.1.5.3. Generation and Distribution of Other Keying Material
3662   Keys are often generated in conjunction with or used with other keying material. This other
3663   keying material shall be protected in accordance with Sec. 5.2. Table 8 specifies the types of
3664   protection required for keying material other than keys.

3665   7.1.5.3.1.        Algorithm Parameters
3666   Algorithm parameters are used by some asymmetric-key algorithms to generate key pairs
3667   and in the execution of their basic functions. Algorithm parameters may be provided in the
3668   algorithm specifications, distributed in the same manner as the public keys with which they
3669   are associated, or be made available at some accessible location. Assurance of the validity of


                                                        112


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

3670   the algorithm parameters shall be obtained prior to use, either by a trusted entity that
3671   vouches for the parameters (e.g., a CA) or by the entities that use them. Assurance of
3672   algorithm-parameter validity is addressed in [SP 800-56A], [SP 800-56B], [FIPS 203] (for key-
3673   agreement and key-encapsulation schemes), and [SP 800-89] (for digital signatures).
3674   Obtaining this assurance should be addressed in a CA’s certification practices statement or
3675   an organization’s security plan.

3676   7.1.5.3.2.        Initialization Vectors (IVs)
3677   Initialization vectors (IVs) are used by symmetric-key algorithms in several modes of
3678   operation for encryption and decryption, for authentication, or both. The criteria for the
3679   generation and use of IVs are provided in the SP 800-38 series of publications. IVs shall be
3680   protected as specified in Section 5.1.2. IVs may be distributed in the same manner as their
3681   associated keys or may be distributed with the information that uses the IVs as part of the
3682   cryptographic mechanism.

3683   7.1.5.3.3.        Shared Secrets Generated During Key Agreement
3684   Shared secrets are computed during the execution of an asymmetric key-agreement scheme
3685   and are subsequently used to derive keying material. Shared secrets are generated as
3686   specified by an appropriate key-agreement scheme (see [SP 800-56A] and [SP 800-56B]) and
3687   shall not be distributed nor used directly as keying material.

3688   7.1.5.3.4.        Seeds
3689   Seeds are used to initialize a deterministic algorithm within an RBG or to generate keying
3690   material from a value provided by an RBG. Approved RBGs are specified in the SP 800-90
3691   series of publications, and each RBG includes a deterministic component that is referred to
3692   as a Deterministic Random Bit Generator (DRBG). RBGs depend on the introduction of truly
3693   random bits that are used to generate the seeds for:
3694       •    Initializing a DRBG. These seeds must be kept secret. An initialized DRBG is often used
3695            to generate keys and other values that require unpredictability. The seeds themselves
3696            shall not be used for any purpose other than as DRBG input.
3697       •    Otherwise determining keying material in accordance with an algorithm specification.
3698   The output of an RBG is sometimes used as a seed to generate or regenerate keying material
3699   rather than initialize a DRBG. In this case, the seed needs to be protected to ensure that the
3700   resulting keying material remains secure.

3701   7.1.5.3.5.        Other Public and Secret Information
3702   Public and secret information may be used during the seeding of the DRBG within an RBG
3703   (see Sec. 7.1.5.3.4) or during the generation or establishment of keying material (see [SP 800-
3704   56A], [SP 800-56B], [SP 800-108], and [SP 800-227]). Public information may be distributed,


                                                        113


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                  Recommendation for Key Management
       December 2025                                                                      Part 1 — General

3705   but secret information shall be protected in the same manner as a private or secret key during
3706   distribution.

3707   7.1.5.3.6.        Intermediate Results
3708   Intermediate results occur during computation using cryptographic algorithms. These results
3709   shall not be distributed as or with the keying material.

3710   7.1.5.3.7.        Random Bits/Numbers
3711   Random bits (or numbers) are used for many purposes, including the generation of keys,
3712   nonces, seeds for generating other values, and the issuing of challenges during the execution
3713   of communication protocols. Random bits may be distributed, but whether confidentiality
3714   protection is required depends on the context in which the random bits are used.

3715   7.1.5.3.8.        Passwords
3716   Passwords are used for identity authentication, authorization, and, in some cases, to derive
3717   keying material (see [SP 800-132]). Passwords may be distributed, but their protection during
3718   distribution shall be consistent with the protection required for their use. For example, if the
3719   password will be used to access cryptographic keys that are used to provide 128 bits of
3720   security strength when protecting data, then the password needs to be provided with at least
3721   128 bits of protection as well. Poorly selected passwords may not provide the required
3722   amount of protection for key access and are potentially the weak point of the process (i.e., it
3723   may be far easier to guess the password than to attempt to “break” the cryptographic
3724   protection used on the password). It is the responsibility of users and organizations to select
3725   passwords that provide the requisite amount of protection for the keys they protect.

3726   7.1.6. Key Registration Function
3727   Key registration results in the binding of keying material to information associated with a
3728   particular entity. Keys that would be registered include the public key of an asymmetric key
3729   pair and the symmetric key used to bootstrap an entity into a system. Normally, keys
3730   generated during communications (e.g., using key-agreement schemes, key encapsulation,
3731   or key derivation functions) would not be registered. Information provided during
3732   registration typically includes the identifier of the entity associated with the keying material
3733   (i.e., the owner) and the intended use of the keying material (e.g., as a signing key or data-
3734   encryption key). Additional information may include authorization information, specify a level
3735   of trust, or indicate where the key is stored (e.g., its physical location, internet address, device
3736   identity).
3737   The binding is performed after the entity’s identity has been authenticated by a means that
3738   is consistent with the system policy (see Sec. 7.1.1). The binding provides assurance to the
3739   community at large that the keying material is used by the correct entity in the correct



                                                        114


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

3740   application. The binding is often cryptographic to create a strong association between the
3741   keying material and the entity. A trusted third party (e.g., Kerberos realm server or PKI CA)
3742   performs the binding. Identifiers issued by a trusted third party shall be unique to that party.
3743   When a Kerberos realm server performs the binding, a symmetric key is stored on the server
3744   with the corresponding metadata. In this case, the registered keying material is maintained
3745   in secure storage (i.e., the keys are provided with confidentiality and integrity protection
3746   during storage). When a CA performs the binding, the public key and associated information
3747   (e.g., algorithm parameters and some metadata, often called attributes) are placed in a
3748   public-key certificate that is digitally signed by the CA. In this case, the registered keying
3749   material may be made publicly available.
3750   When a CA provides a certificate for a public key, the public key shall be verified to ensure
3751   that it is associated with the private key known by the purported owner of the public key.
3752   This provides assurance of possession (i.e., POP). When POP is used to obtain assurance of
3753   possession, the assurance shall be accomplished, as specified in Sec. 7.1.5.1.1.2.
3754   Registered keys and certificates shall be included in an inventory (see Sec. 8.2).

3755   7.2. Operational Phase
3756   Keying material used during the cryptoperiod of a key is often stored for access as needed.
3757   During storage, the keying material and other key information shall be protected, as specified
3758   in Sec. 5.2.2. During normal use, the key information is stored in the device or module that
3759   uses that information or on an immediately accessible storage media. Keying material is
3760   acquired from storage when required for operational use and not present in active memory
3761   within the device or module.
3762   To provide continuity of operations when the keying material becomes unavailable for use
3763   from normal operational storage during its cryptoperiod (e.g., because the material is lost or
3764   corrupted), keying material may need to be recoverable. If an analysis of system operations
3765   indicates that the keying material needs to be recoverable, then the keying material shall be
3766   backed up (see Sec. 7.2.2.1) or archived (see Sec. 7.3.1), or the system shall be designed to
3767   allow reconstruction (e.g., re-derivation) of the keying material. Retrieving or reconstructing
3768   keying material from an archive or backup storage is commonly known as key recovery (see
3769   Sec. 7.2.2.2).
3770   At the end of a key’s cryptoperiod, a new key needs to be available to replace the old key if
3771   operations are to continue. This can be accomplished by re-keying (see Sec. 7.2.3.1) or by key
3772   derivation (see Sec. 7.2.4). A key should be destroyed as soon as that key is no longer needed
3773   to reduce the risk of exposure; when destroyed, the key shall be destroyed in accordance
3774   with Sec. 7.3.4.

3775   7.2.1. Normal Operational Storage Function
3776   One objective of key management is to facilitate the operational availability of keying
3777   material for standard cryptographic purposes. Usually, a key remains operational until the


                                                        115


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

3778   end of the key’s cryptoperiod (i.e., the expiration date). During normal operational use,
3779   keying material is available in the device or module (e.g., in RAM) or in an immediately
3780   accessible storage media (e.g., on a local hard drive).

3781   7.2.1.1. Cryptographic Module Storage
3782   Keying material may be stored in a cryptographic module that adds, checks, or removes the
3783   cryptographic protection on information. The storage of the keying material shall be
3784   consistent with both Sec. 5.2.2 and [FIPS 140-3]. Additional storage-media discussions are
3785   provided in Sec. 5.2.2.7.

3786   7.2.1.2. Immediately Accessible Storage Media
3787   Keying material may need to be stored for normal cryptographic operations on an
3788   immediately accessible storage media (e.g., a local hard drive) during the cryptoperiod of the
3789   key. The storage requirements of Sec. 5.2.2 shall apply to this keying material.

3790   7.2.2. Continuity of Operations Function
3791   Keying material can become lost or unusable due to hardware damage, corruption, the loss
3792   of program or data files, system policy, or configuration changes. To maintain continuity, it is
3793   often necessary for users and/or administrators to be able to recover keying materials from
3794   backup storage. However, if operations can be continued without the backup of keying
3795   material (e.g., by re-keying) or the keying material can be recovered or reconstructed without
3796   being saved, it may be preferable not to save the keying material to lessen the possibility of
3797   a compromise of the keying material or other cryptographically related information.
3798   The compromise of keying material affects the continuity of operations (see Sec. 8.5). When
3799   keying material is compromised, the continuity of operations requires the establishment of
3800   entirely new keying material (see Sec. 7.1.5) following an assessment of the keying material
3801   that was affected. All affected keying material needs to be replaced.

3802   7.2.2.1. Backup Storage
3803   The backup of keying material on an independent, secure storage media provides a source
3804   for key recovery (see Sec. 7.2.2.2). Backup storage is used to store copies of key information
3805   that is also currently available in normal operational storage during a key’s cryptoperiod (i.e.,
3806   in the cryptographic module or on an immediately accessible storage media; see Sec. 7.2.1.1).
3807   Not all keys need to be backed up, and others need additional protections. For example,
3808   additional protection needs to be provided for the backup of keys for stateful hash-based
3809   signatures due to the high risk associated with reusing the one-time private keys. The storage
3810   requirements of Sec. 5.2.2 apply to keying material that is backed up.
3811   Keying material maintained in backup storage should remain in storage for at least as long as
3812   the same keying material is maintained for normal operational use (see Sec. 7.2.1). When no


                                                        116


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                       Recommendation for Key Management
       December 2025                                                                                           Part 1 — General

3813   longer needed for normal operational use, the keying material and other related information
3814   should be removed from backup storage. When removed from backup storage, all traces of
3815   the information in backup storage shall be destroyed in accordance with Sec. 7.3.4. 41
3816   Table 10 and Table 11 provide guidelines for backing up each type of keying material and
3817   other related information. An “OK” indicates that storage is permissible but not necessarily
3818   required. The final determination for backup should be made based on the application in
3819   which the keying material is used. A detailed discussion about the backup of each type of key
3820   and other key information is provided in Appendix B.1.3.
3821                                                          Table 10. Backup of keys

                             Type of Key                                                         Backup
                                                                    Digital Signatures
        1. Private signature key                                     No (in general); support for non-repudiation would be in
                                                                     question. However, backup may be warranted in some cases,
                                                                     such as a CA’s private signing key. When required, any backed
                                                                     up keys shall be stored under the owner’s control.
        2. Public signature-verification key                         OK; its presence in a public-key certificate that is available
                                                                     elsewhere may be sufficient.
                                                                     Authentication
        3. Symmetric authentication key                              OK
        4. Private authentication key                                OK if required by an application.
        5. Public authentication key                                 OK if required by an application.
                                                                Random Bit Generation
        6. Random number generation key                              Not necessary and may not be desirable, depending on the
                                                                     application.
                                                                      Key Derivation
        7. Symmetric key-derivation                                  OK
                                                           Key Establishment (automated)
        8. Symmetric key-wrapping/unwrapping                         OK
          key
        9. Public key-transport key                                  OK; its presence in a public-key certificate that is available
                                                                     elsewhere may be sufficient.
        10, Private key-transport key                                OK
        11. Symmetric key-agreement key                              OK
        12. Public static key-agreement key                          OK; its presence in a public-key certificate that is available
                                                                     elsewhere may be sufficient.
        13. Private static key-agreement key                         OK
        14. Public ephemeral key-agreement key                       OK
          15. Private ephemeral key-agreement key                    OK



       41 A discussion of backup and recovery is provided in [ITLBulletin].




                                                                              117


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                           Recommendation for Key Management
       December 2025                                                                               Part 1 — General

                         Type of Key                                                Backup
        16. Public static encapsulation key             OK; its presence in a public-key certificate that is available
                                                        elsewhere may be sufficient.
        17. Private static decapsulation key            OK
        18. Public ephemeral encapsulation key          No; it is only to be used for a single key-encapsulation process.
        19. Private ephemeral decapsulation key         OK until no longer required for decapsulation of the (only)
                                                        encapsulated keys.
                                                  Data Encryption/Decryption
        20. Symmetric data encryption/decryption        OK
            key (data in transit)
        21. Symmetric data encryption/decryption        OK
            key (data at rest)
                                   Key Storage (for operational, backup, and archive storage)
        22. Symmetric key-wrapping/unwrapping           OK
            key
        23. Public key-wrapping key                     OK
        24.Private key-unwrapping key                   OK
        25. Public encapsulation key                    OK
        26.Private decapsulation key                    OK
                                                         Authorization
        27. Symmetric authorization key                 OK
        28. Private authorization key                   OK
        29. Public authorization key                    OK; its presence in a public-key certificate that is available
                                                        elsewhere may be sufficient.

3822                                    Table 11. Backup of other related information

                    Type of Keying Material                                         Backup?
        Algorithm parameters                            OK
        Initialization vector                           OK if necessary
        Shared secret                                   No
        Seed                                            No for DRBG initialization; otherwise, OK
        Other public information                        OK
        Other secret information                        OK
        Intermediate results                            No
        Key-control information/metadata (e.g., IDs     OK
        or purpose)
        Random number                                   Depends on the application or use of the random number.
        Passwords                                       OK when used to derive keys or to detect the reuse of
                                                        passwords; otherwise, No.
        Audit information                               OK




                                                             118


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                        Recommendation for Key Management
       December 2025                                                                                            Part 1 — General

3823   7.2.2.2. Key Recovery Function
3824   Keying material that is in active memory or stored in normal operational storage may
3825   sometimes be lost or corrupted (e.g., from a system crash or power fluctuation). Some of the
3826   keying material is needed to continue operations and cannot be easily replaced. An
3827   assessment needs to be made about which keying material needs to be preserved for possible
3828   recovery at a later time.
3829   The decision about whether key recovery is required should be made on a case-by-case basis.
3830   The decision should be based on:
3831        •    The type of key (e.g., private signature key or symmetric data-encryption key),
3832        •    The application in which the key will be used (e.g., interactive communications or file
3833             storage),
3834        •    Whether the key is “owned” by the local entity (e.g., a private key), owned by another
3835             entity (e.g., the other entity’s public key), or shared (e.g., a symmetric data-encryption
3836             key shared by two entities),
3837        •    The role of the entity in a communication (e.g., sender or receiver), and
3838        •    The algorithm or computation in which the key will be used (e.g., whether the entity
3839             has the necessary information to perform a given computation if the key were to be
3840             recovered). 42
3841   The factors involved in a decision for or against key recovery should be carefully assessed.
3842   The trade-offs are concerned with the continuity of operations versus the risk of possibly
3843   exposing the keying material and the information it protects if control of the keying material
3844   is lost. If it is determined that a key needs to be recovered, and the key is still active (e.g., the
3845   cryptoperiod of the key has not expired, and the key has not been compromised), the key
3846   could be replaced rather than recovered in order to limit the exposure of the data protected
3847   by the lost key (see Sec. 7.2.3).
3848   Issues associated with key recovery and discussions about whether different types of
3849   cryptographic material need to be recoverable are provided in Appendix B.

3850   7.2.3. Key Change Function
3851   Key change is the replacement of a key with another key that performs the same function as
3852   the original key. There are several reasons for changing a key, such as:
3853        1. The key may have been compromised.
3854        2. The key’s cryptoperiod may be nearing expiration.
3855        3. It may be desirable to limit the amount of data protected with any given key.


       42 This could be the case when performing a key-establishment process for some key-establishment schemes (see [SP 800-56A] and [SP 800-

       56B]).



                                                                        119


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

3856   7.2.3.1. Re-Keying
3857   If a new key is generated in a manner that is entirely independent of the “value” of the old
3858   key, the process is known as re-keying. Replacement shall be accomplished using one of the
3859   key-establishment methods discussed in Sec. 7.1.5. Re-keying is used when a key has been
3860   compromised (provided that the re-keying scheme itself is not compromised) or when the
3861   cryptoperiod has expired or is nearing expiration.

3862   7.2.3.2. Key Update Function
3863   If the “value” of a new key depends on the value of the old key, the process is known as key
3864   update (e.g., the current key is modified in some way to create a new key). Key update is a
3865   special case of key derivation (see Sec. 7.2.4) in which a derived key replaces the key used to
3866   derive it. For example, suppose that K1 is used as an encryption key. When K1 needs to be
3867   replaced, it is used to derive K2. K2 is then used as the new encryption key until it is replaced
3868   by K3, which is derived from K2.
3869   Key update could result in a security exposure if an adversary obtains a key in the chain of
3870   derived keys and knows the update process that was used. Keys derived from a compromised
3871   key in the chain could then be easily determined.
3872   Federal applications shall not use key update (also see [SP 800-152]).

3873   7.2.4. Key Derivation Methods
3874   Cryptographic keys may be derived from a secret value. The secret value and other
3875   information are input into a key-derivation method (e.g., a key-derivation function) that
3876   outputs the required keys). The key-derivation method shall be nonreversible (i.e., a one-way
3877   function) so that the secret value cannot be determined from the derived keys. In addition, it
3878   shall not be possible to determine a derived key from other derived keys. It should be noted
3879   that the strength of a derived key is no greater than the strength of the derivation algorithm
3880   and the secret value from which the key is derived.
3881   Three commonly used key-derivation cases are discussed below.
3882       1. Two parties derive common keys from a common shared secret. This approach is used
3883          in the key-agreement techniques specified in [SP 800-56A] and [SP 800-56B]. The
3884          security of this process depends on the security of the shared secret and the specific
3885          key-derivation method used. If the shared secret is known, the derived keys can be
3886          determined. A key-derivation method specified or allowed in [SP 800-56C] shall be
3887          used for this purpose. These derived keys may be used to provide the same
3888          confidentiality, identity authentication, and source authentication services as
3889          randomly generated keys. The security strength of the derived keys is determined by
3890          the scheme and key pairs used to generate the shared secret.
3891       2. Keys derived from a key-derivation key (master key). This is often accomplished by
3892          using a secret key-derivation key and other known secret or public information as


                                                        120


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

3893            input to a function that generates the keys. One of the key-derivation functions
3894            defined in [SP 800-108] or [SP 800-135] shall be used for this purpose. The security of
3895            this process depends on the security of the key-derivation key and the key-derivation
3896            method. If the key-derivation key is known by an adversary, any of the derived keys
3897            can be generated. Therefore, keys derived from a key-derivation key are only as
3898            secure as the key-derivation key itself. If the key-derivation key is kept secret, the
3899            derived keys may be used in the same manner as randomly generated keys.
3900       3. Keys derived from a password. A user-generated password is inherently less random
3901          (i.e., has lower entropy) than is required for a cryptographic key. That is, the number
3902          of passwords that are likely to be used to derive a key is significantly smaller than the
3903          number of keys that are possible for a given key size. To increase the difficulty of
3904          exhaustively searching the likely passwords, a key-derivation function is iterated a
3905          large number of times. The key is derived using a password and other known secret
3906          or public information as input to the key-derivation function. The security of the
3907          derived key depends on the security of the password and the key-derivation process.
3908          If the password is known or can be guessed, then the corresponding derived key can
3909          be generated. Therefore, keys derived in this manner are likely to be less secure than
3910          randomly generated keys or keys derived from a shared secret or secret key-
3911          derivation key. For storage applications, one of the key-derivation functions specified
3912          in [SP 800-132] shall be used to derive keys from passwords. For non-storage
3913          applications, keys derived in this manner shall not be used for general encryption but
3914          may sometimes be used for identity and source authentication purposes.

3915   7.3. Post-Operational Phase
3916   During the post-operational phase, keying material is no longer in operational use, but access
3917   to the required keying material may still be possible for processing already protected
3918   information.

3919   7.3.1. Key Archive and Key Recovery Functions
3920   A key archive is a repository that contains keys and their associated information (i.e., key
3921   information) for recovery beyond the cryptoperiod of the keys. Not all keys need to be
3922   archived. An organization’s security plan should discuss key archiving (see [SP 800-57p2]).
3923   The key archive shall continue to provide the appropriate protections for each key and any
3924   other related information in the archive, as specified in Sec. 5.2.2. The archive will require a
3925   strong access-control mechanism to limit access to the archive information to only authorized
3926   entities. When key information is entered into the archive, it is often timestamped so that
3927   the date of entry can be determined. This date may itself be cryptographically protected so
3928   that it cannot be changed without detection.
3929   If a key must be recoverable (e.g., after the end of its cryptoperiod), either the key shall be
3930   archived, or the system shall be designed to allow reconstruction (e.g., re-derivation) of the
3931   key from archived information. Retrieving the key from archive storage or by reconstruction


                                                        121


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                           Recommendation for Key Management
       December 2025                                                                               Part 1 — General

3932   is commonly known as key recovery. The archive shall be maintained by a trusted party (e.g.,
3933   the organization associated with the key or a trusted third party).
3934   Archived key information shall be stored separately from operational data, and multiple
3935   copies of archived key information should be provided in physically separate locations (i.e.,
3936   the key archive should be backed up). For critical information that is protected (i.e.,
3937   encrypted, wrapped, or encapsulated) under archived keys, it may be necessary to back up
3938   the archived keys and store multiple copies of these archived keys in separate locations.
3939   When archived, keys should be archived prior to the end of the key’s cryptoperiod. For
3940   example, it may be prudent to archive the key when it is activated. When no longer required,
3941   the key shall be destroyed in accordance with Sec. 7.3.4.
3942   The confidentiality of archived key information is provided by an archive-confidentiality key
3943   (i.e., one or more encryption, wrapping, or encapsulation keys that are used exclusively to
3944   protect the confidentiality of archived key information), by another key that has been
3945   archived, or by a key that may be derived from an archived key. The algorithm with which an
3946   archive-confidentiality key is used may also provide integrity protection for the archived
3947   information.
3948   When an archive-confidentiality key and its associated algorithm do not also provide integrity
3949   protection for the archived information, integrity protection shall be provided by a separate
3950   archive-integrity key (i.e., one or more authentication or digital signature keys that are used
3951   exclusively for the archive) or by another key that has been archived.
3952   When the confidentiality and integrity protection of the archived key information are
3953   provided using separate processes, the archive-confidentiality key and archive-integrity key
3954   shall be different from each other (e.g., independently generated) and shall be protected in
3955   the same manner as their key type (see Sec. 5.1.1). These two services could also be provided
3956   using a single cryptographic algorithm and a single key.
3957   Table 12 and Table 13 indicate the appropriateness of archiving keys and other
3958   cryptographically related information. An “OK” in column 2 (Archive?) indicates that archiving
3959   is permissible but not necessarily required. Column 3 (Retention period) indicates the
3960   minimum time that the key should be retained in the archive. Additional advice on the
3961   storage of keying material in archive storage is provided in Appendix B.1.3.
3962                                               Table 12. Archive of keys

        Type of Key                                     Archive?        Retention period (minimum)
                                                         Digital Signatures
        1. Private signature key                          No
        2. Public signature-verification key                            Until no longer required to verify data signed
                                                          OK
                                                                        with the corresponding private signature key
                                                          Authentication
        3. Symmetric authentication key                                 Until no longer needed to authenticate data or
                                                          OK
                                                                        an identity
        4. Private authentication key                     No


                                                               122


---

NIST SP 800-57pt1r6 ipd (Initial Public Draft)                            Recommendation for Key Management
December 2025                                                                                Part 1 — General

 Type of Key                                      Archive?       Retention period (minimum)
 5. Public authentication key                        OK
                                                 Random Bit Generation
 6. Symmetric random number
                                                     No
 generator key
                                                    Key Derivation
 7. Symmetric key-derivation/master         OK, if needed to     Until no longer needed to derive other keys
 key                                       derive other keys
                                           for archived data
                                         Key Establishment (automated)
 8.Symmetric key-                                                Until no longer needed to unwrap keys that
                                                     OK
 wrapping/unwrapping key                                         were wrapped by this key
 9. Public key-transport key                         OK          No real use after its cryptoperiod
 10. Private key-transport key                                   Until no longer needed to decrypt keys
                                                     OK          encrypted by the corresponding public key-
                                                                 transport key
 11. Symmetric key-agreement key                                 Until no longer useful for determining the key
                                                     OK
                                                                 agreed upon
 12. Public static key-agreement key                             Until no longer needed to reconstruct keying
                                                     OK
                                                                 material
 13. Private static key-agreement                                Until no longer useful for determining the key
                                                     OK
 key                                                             agreed upon
 14. Public ephemeral key-                                       Until no longer useful for determining the key
                                                     OK
 agreement key                                                   agreed upon
 15. Private ephemeral key-
                                                     No
 agreement key
 16. Public static encapsulation key                 OK          No real use after its cryptoperiod
 17. Private static decapsulation key                            Until no longer needed to decapsulate keys that
                                                     OK          were encapsulated by the corresponding public
                                                                 static encapsulation key
 18. Public ephemeral encapsulation
                                                     No
 key
 19. Private ephemeral                                           Until no longer needed to decapsulate the key
 decapsulation key                                   OK          that was encapsulated by the corresponding
                                                                 public ephemeral encapsulation key
                                           Data Encryption/Decryption
 20. Symmetric data
 encryption/decryption keys:
 (data in transit)                                               Until no longer needed to decrypt data that was
                                                     OK
 21. Symmetric data                                              encrypted by this key
 encryption/decryption key:
 (data at rest)
                            Key Storage (for operational, backup, and archive storage)




                                                          123


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                           Recommendation for Key Management
       December 2025                                                                               Part 1 — General

        Type of Key                                     Archive?       Retention period (minimum)
        22. Symmetric key-                                             Until no longer needed to unwrap keys that
                                                          OK
        wrapping/unwrapping key                                        were wrapped by this key
        23. Public key-wrapping key                       OK           No real use after its cryptoperiod
                                                                       Until no longer needed to unwrap keys that
        24.Private key-unwrapping key                     OK           were wrapped by the corresponding public key-
                                                                       wrapping key
        25. Public encapsulation key                      OK           No real use after its cryptoperiod
                                                                       Until no longer needed to decapsulate keys that
        26.Private decapsulation key                      OK           were encapsulated by the corresponding public
                                                                       encapsulation key
                                                           Authorization
        Symmetric authorization key                       No
        Private authorization key                         No
        Public authorization key                                       No real use after the cryptoperiod of the
                                                          OK
                                                                       corresponding private authorization key

3963                                    Table 13. Archive of other related information

                   Type of Key                          Archive?                  Retention period (minimum)
        Algorithm parameters                                           Until all keying material, signatures, and signed
                                                          OK           data using the algorithm parameters are
                                                                       removed from archives
        Initialization vector                       OK; normally       Until no longer needed to process the protected
                                                   stored with the     data
                                                      protected
                                                     information
        Shared secret                                     No
        Seed                                              No
        Other public information                                       Until no longer needed to process data using the
                                                          OK
                                                                       public information
        Other secret information                                       Until no longer needed to process data using
                                                          OK
                                                                       the secret information
        Intermediate result                               No
        Key-control information/metadata                               Until the associated key is removed from the
                                                          OK
        (e.g., IDs, purpose)                                           archive
        Random number                                                  Depends on the application or use of the
                                                                       random number
        Password                                  OK when used to      Until no longer needed to derive or rederive
                                                  derive keys or to    keys or to detect password reuse
                                                 detect the reuse of
                                                    passwords;
                                                   otherwise, No
        Audit information                                OK            Until no longer needed

3964   The recovery of archived keying material may be required to remove (e.g., decrypt) or check
3965   (e.g., verify a digital signature or MAC) the cryptographic protections on other archived data.
3966   Recovered keys shall not be used to apply cryptographic protection if the cryptoperiod (or
3967   originator-usage period) of those keys has expired. The key-recovery process results in


                                                               124


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                       Recommendation for Key Management
       December 2025                                                                                           Part 1 — General

3968   retrieving or reconstructing the desired keying material that is retrieved from archive storage
3969   to perform the required cryptographic operation. Immediately after completing this
3970   operation, the keying material shall be erased from the cryptographic process 43 for which it
3971   was recovered (i.e., it shall not be used for normal operational activities). However, the key
3972   shall be retained in the archive (see Sec. 7.3.4) as long as needed. Further guidelines
3973   concerning key-recovery issues is provided in Appendix B.

3974   7.3.2. Entity De-Registration Function
3975   The entity de-registration function removes the authorizations of an entity to participate in a
3976   security domain. When an entity ceases to be a member of a security domain, the entity shall
3977   be de-registered. De-registration is intended to prevent other entities from relying on or using
3978   the de-registered entity’s keying material (e.g., a symmetric key shared with the de-registered
3979   entity).
3980   All records of the entity and the entity’s associations shall be marked to indicate that the
3981   entity is no longer a member of the security domain, but the records should not be deleted,
3982   even though the key itself has been destroyed. To reduce confusion and unavoidable human
3983   errors, identification information associated with the de-registered entity should not be re-
3984   used (at least for a period of time). For example, if a “John Wilson” retires and is de-registered
3985   on Friday, the identification information assigned to his son “John Wilson,” who is hired the
3986   following Monday, should be different.

3987   7.3.3. Key De-Registration Function
3988   Registered keying material may be associated with the identity of a key owner, owner
3989   information (e.g., email address), role, or authorization information. When the keying
3990   material is no longer needed or the associated information becomes invalid, the keying
3991   material should be de-registered (i.e., all records of the keying material and its associations
3992   should be marked to indicate that the key is no longer in use) by the appropriate trusted third
3993   party. In general, this will be the trusted third party that registered the key (see Sec. 7.1.6).
3994   Keying material should be de-registered when the information associated with an entity is
3995   modified. For example, if an entity’s email address is associated with a public key and the
3996   entity’s address changes, the keying material should be de-registered to indicate that the
3997   associated information has become invalid. Unlike the case of a key compromise, if the
3998   cryptoperiod of the key has not expired, the entity could safely re-register the public key after
3999   modifying the entity’s information through the entity registration process (see Sec. 7.1.1).
4000   When a registered cryptographic key is compromised, that key and any associated keying
4001   material shall be de-registered. When the compromised key is the private key of a key pair,
4002   the public key shall also be revoked (Sec. 7.3.5). The de-registered key shall not be re-
4003   registered.

       43 For example, an archived symmetric key could be recovered to decrypt a single message or file or could be used to decrypt multiple

       messages or files, all of which were encrypted using that key during its originator-usage period.



                                                                       125


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                        Recommendation for Key Management
       December 2025                                                                                            Part 1 — General

4004   If the registration information associated with a key pair is changed but the private key has
4005   not been compromised, the public key should be revoked with an appropriate reason code
4006   (see Sec. 7.3.5). In this case, the key may be re-registered if the cryptoperiod has not expired.

4007   7.3.4. Key Destruction Function
4008   When copies of cryptographic keys are made, care should be taken to provide for their
4009   eventual destruction (e.g., the identity of sharing parties could be recorded in the key’s
4010   metadata). All copies of a private or secret (symmetric) key shall be destroyed as soon as they
4011   are no longer required (e.g., for archival or reconstruction activity) to minimize the risk of a
4012   compromise. Secret and private keys shall be destroyed in a manner that removes all traces
4013   of the keys so that they cannot be recovered by either physical or electronic means. 44 Public
4014   keys may be retained or destroyed as desired.

4015   7.3.5. Key Revocation Function
4016   Key revocation is used if 1) the authorized use of a key needs to be terminated prior to the
4017   end of the established cryptoperiod of that key, or 2) a key whose usage period or
4018   cryptoperiod has expired has been compromised. A key may be revoked for administrative
4019   reasons (e.g., the key’s owner has left the organization or a device containing the key has
4020   been removed from service), or it may be revoked on an emergency basis if there is reason
4021   to believe that it may have been disclosed to or otherwise accessed by an unauthorized
4022   entity. In either case, a cryptographic key should be revoked as soon as feasible after the
4023   need for revocation has been determined.
4024   Entities that have been, are, or would be using the key (e.g., relying parties) need to be
4025   notified that the key has been revoked.
4026        •    When a (secret) symmetric key is revoked, all entities sharing the key need to be
4027             notified (e.g., using a compromised key list [CKL] or a specific notification provided to
4028             each entity sharing the key).
4029        •    In the case of asymmetric key pairs, the revocation refers to the private key. However,
4030             when public-key certificates are used, the certificate containing the public key
4031             corresponding to the private key is revoked, and relying parties are notified using, for
4032             example, certificate revocation lists (CRLs) or the Online Certificate Status Protocol
4033             (OCSP).
4034   The notification could be provided by actively sending a notification to all entities that might
4035   be using the revoked key or by allowing the entities to request the key’s status (i.e., a “push”
4036   or a “pull” of the status information). The notification should include a complete
4037   identification of the key (excluding the key itself), the date and time of revocation, and the

       44
         A simple deletion of the keying material might not completely obliterate the information. For example, erasing the information might
       require overwriting that information multiple times with other unrelated information, such as random bits or all zero or one bits. Keys
       stored in memory for a long time can become “burned in.” This can be mitigated by splitting the key into components that are frequently
       updated (see [DiCrescenzo]).



                                                                        126


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
       December 2025                                                                     Part 1 — General

4038   reason for revocation when appropriate (e.g., a key compromise). Based on the revocation
4039   information provided, other entities could then determine how they will treat information
4040   that was protected by the revoked key.
4041   For example, if a public signature-verification key is revoked because an entity left an
4042   organization, it may be appropriate to honor all signatures created prior to the revocation
4043   date (i.e., to continue to verify those signatures and accept them as valid if the verification is
4044   successful). If a signing private key is compromised, resulting in the revocation of the
4045   corresponding public key, an assessment needs to be made about whether information
4046   signed prior to the date in the revocation notice would be considered valid. As another
4047   example, a symmetric key that is used to generate MACs may be revoked so that it will not
4048   be used to generate MACs on new information. However, the key may be retained so that
4049   the MAC on archived documents can be verified.
4050   The details for key revocation should reflect the life cycle for each particular key. If a key is
4051   used in a pairwise situation (e.g., two entities communicating using the same symmetric
4052   encryption key), the entity revoking the key shall inform the other entity of the revocation. If
4053   the key has been registered with an infrastructure (e.g., a particular PKI), the entity revoking
4054   the key cannot always directly inform the other entities that may rely upon that key. Instead,
4055   the entity revoking the key shall inform the infrastructure that the key needs to be revoked
4056   (e.g., using a certificate revocation request). The infrastructure shall respond by revoking and
4057   de-registering the key (see Sec. 7.3.3).
4058   In a PKI, key revocation is commonly achieved by including the certificate in a list of revoked
4059   certificates (i.e., in a CRL). If the PKI uses an online status mechanism (e.g., the Online
4060   Certificate Status Protocol in RFC 2560 [RFC 2560]), revocation is achieved by informing the
4061   appropriate certificate status servers. For example, when a private key is compromised, the
4062   corresponding public-key certificate shall be revoked as soon as possible. Certificate
4063   revocation because of a key compromise indicates that the binding between the owner and
4064   the key can no longer be trusted. Relying parties should not accept the certificate without
4065   seriously considering the risks and consulting the organization’s policy about this situation.
4066   Other revocation reasons indicate that even though the original binding may still be valid,
4067   and the key was not compromised, the use of the public key in the certificate should be
4068   terminated. Again, the relying party should consult the organization’s policy on this issue.
4069   In a symmetric-key system, key revocation could theoretically be achieved by simply deleting
4070   the key from the server’s storage. Key revocation for symmetric keys is more commonly
4071   achieved by adding the key to a compromised key list, which helps satisfy auditing and
4072   management requirements.

4073   7.4. Destroyed Phase
4074   The key is no longer available. All records of its existence may have been deleted, although
4075   this is not required. Some organizations may require the retention of certain metadata
4076   elements for audit purposes. For example, if a copy of an ostensibly destroyed key is found
4077   in an uncontrolled environment or is later determined to have been compromised, records


                                                        127


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)             Recommendation for Key Management
       December 2025                                                                 Part 1 — General

4078   of the key’s identifier, its type, and its cryptoperiod may be helpful in determining what
4079   information was protected under the key and how best to recover from the compromise.
4080   In addition, by keeping a record of the metadata of both destroyed and compromised keys,
4081   one can determine which keys transitioned through a normal life cycle and which were
4082   compromised at some time during their life cycle. Thus, protected information that is linked
4083   to key names that went through the normal life cycle may still be considered secure, provided
4084   that the security strength of the algorithm remains sufficient. However, any protected
4085   information that is linked to a key name that has been compromised may itself be
4086   compromised.
4087




                                                        128


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

4088   8. Additional Considerations
4089   Many aspects of key management can be handled by a key-management system that is
4090   designed to manage cryptographic keys and their metadata. An automated key-management
4091   system may be used to oversee, automate, and secure the key-management process.
4092   However, there are additional considerations when designing and operating such a system.
4093   Among these are controlling access to the system by authenticating the individuals
4094   requesting access and verifying their authorization to do so (see Sec. 8.1), creating and
4095   maintaining an inventory of keys and certificates to monitor when a key or certificate needs
4096   to be replaced and who is responsible for key and certificate replacement (see Sec. 8.2),
4097   assigning responsibilities and monitoring system activities (see Sec. 8.3), auditing the
4098   implementation and performance of the system and examining audit logs for irregularities
4099   (see Sec. 8.4), and ensuring system survivability (see Sec. 8.5).

4100   8.1. Access Control and Identity Authentication
4101   An access control system is needed to ensure that every key and metadata management
4102   function can only be initiated in response to a request by an authorized entity. When key-
4103   management functions are initiated by an entity, an access control system must ensure that
4104   the entity is authenticated and only performing the requested functions that are authorized
4105   for that entity and that all applicable constraints are satisfied.
4106   The access-control system shall control access to and the initiation of all of its key and
4107   metadata management services and functions, including granting access and permission to
4108   initiate a requested service or function only after verifying the identity and authorization of
4109   the requesting entity to perform the requested service or function.
4110   Providing access control requires a means of identifying the entities accessing the keys.
4111   Commonly used methods include the use of two-factor authentication and digital signature
4112   certificates. These methods require identity proofing of the entities. [SP 800-63], [SP 800-
4113   130], and [SP 800-152] provide further discussions and requirements for access control and
4114   identity authentication.
4115   All access to keys should be recorded in audit logs for periodic and emergency examination,
4116   including both successful and failed access attempts. See Sec. 8.4 for a discussion about
4117   auditing these logs.

4118   8.2. Inventory Management
4119   When using cryptographic mechanisms that employ keys, all long-term keys shall be
4120   inventoried. In the case of symmetric keys, this includes the keys used for the protection of
4121   information in transit and in storage. For asymmetric key pairs, this includes the key pairs
4122   owned by organizational entities (i.e., entities within the organization that are authorized to
4123   use the private key of the key pair). When certificates are issued for the public key of the key
4124   pair, records shall be maintained for those certificates. These records need to be maintained
4125   by an inventory management system.


                                                        129


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                    Recommendation for Key Management
       December 2025                                                                        Part 1 — General

4126   Inventory management is concerned with establishing and maintaining records of the keys
4127   and/or certificates in use, assigning and tracking their owners or sponsors45 (e.g., who or what
4128   they are, where they are located, and how to contact them), monitoring key and certificate
4129   status (e.g., expiration date and whether a key is compromised), and reporting the status to
4130   the appropriate official for remedial action when required.

4131   8.2.1. Key Inventories
4132   A key inventory shall include information about each long-term key (e.g., all or part of the
4133   metadata associated with the key). Unless the inventory is also used for key backup or
4134   archiving, the inventory shall not include secret or private keys but shall include a reference
4135   to the key (e.g., a key identifier or pointer to the location of the key). The information in the
4136   inventory should indicate who/what owns or shares the key, the key type, the algorithm with
4137   which the key is to be used, the key length, how it is used (e.g., the application), and its
4138   expiration date.
4139   Key inventories should be maintained in a central repository or network of mutually trusted
4140   repositories and operated in accordance with a Key-Inventory Policy. 46 If a key is
4141   compromised, the owners or sponsors associated with the compromised key need to be
4142   notified so that remedial actions can be taken, including revoking the key, analyzing the
4143   effects of the compromise, and replacing the key when appropriate. The information in the
4144   inventory can be used to identify who needs to be notified and how to contact them.
4145   If an owner is no longer authorized to use a key (e.g., the owner is a human who left the
4146   organization or a device that was removed from the system), other entities need to be
4147   notified so that further interaction using that key is terminated. If the key is a symmetric key,
4148   the information in the inventory can be used to identify other entities that need to be notified
4149   and how to contact them. If the key is an asymmetric key and PKI certificates are used,
4150   notification is usually accomplished using CRLs.
4151   The key needs to be replaced if its cryptoperiod expires or is about to expire and interactions
4152   between the entities using that key are to continue (i.e., using a replacement key). A key-
4153   inventory management system can be used to monitor cryptoperiods and alert the key
4154   owners or sponsors that keys are about to expire. The inventory management system can
4155   also be used to find keys for algorithms and key lengths that are no longer considered to be
4156   secure in order to arrange for algorithm and/or key replacement.

4157   8.2.2. Certificate Inventories
4158   Certificates are used by communication servers (e.g., TLS, SSH) to provide web applications
4159   and services (e.g., government services, online banking, flight operations, mission-critical
4160   services within an organization), by devices (e.g., routers), and by client applications (e.g.,
4161   browsers) used by the humans who use those communications and services. The certificates

       45 See Sec. 2 for a discussion about owners and sponsors.
       46 See [SP 800-57p2] for additional information.




                                                                   130


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
       December 2025                                                                     Part 1 — General

4162   are used to establish identity, provide public keys to verify signatures on documents and the
4163   integrity of communicated information, and to establish keys to protect communications.
4164   In many cases, certificates have been created and installed without recording the details
4165   associated with them (e.g., who or what device or process is associated with a certificate,
4166   what device or process has the private key, the location of the device or process, or the
4167   certificate’s validity period). As a result, significant outages occur when 1) the certificate
4168   expires and needs to be replaced before business operations can continue, or 2) the private
4169   key is compromised, and the certificate needs to be revoked and replaced before secure
4170   operations can be assured. To facilitate recovery operations and avoid outages in the case of
4171   expired certificates, a certificate shall be inventoried upon creation.
4172   A certificate inventory includes the latest certificates for each entity and information about
4173   each certificate, including the identity of the certificate owner and contact information for
4174   the owner. The private keys associated with the public keys in the certificates shall not be
4175   included in the inventory unless the inventory is also used for key backup or archiving, and
4176   the backup or archiving of the private key is permitted (see Sec. 7.2.2.1 and 7.3.1). Certificate
4177   inventories should be maintained in a central repository or network of mutually trusted
4178   repositories and operated in accordance with a certificate policy. See [SP 800-57p2] for
4179   additional information.
4180   A certificate inventory application shall be used to enter a certificate into the inventory,
4181   monitor certificate validity periods for proactive certificate replacement, detect the use of
4182   algorithms and key lengths that are no longer secure, respond to cryptographic incidents
4183   (e.g., CA compromise), and modify who should be contacted for certificate maintenance.

4184   8.3. Accountability
4185   Accountability has two different aspects with respect to key management: responsibility and
4186   traceability.
4187   Each human involved with key management must be clearly informed about their key-
4188   management responsibilities and held accountable for fulfilling them. This includes the roles
4189   that people are assigned, the key-management responsibilities for those roles, and
4190   monitoring the performance of the individuals assigned to those roles. See [SP 800-130] for
4191   discussions of the different roles and responsibilities associated with key management.
4192   Traceability involves identifying entities that are authorized to generate, access, destroy, or
4193   otherwise use keys; recording who or what actually performs these actions; and auditing the
4194   logs for security violations (see Sec. 8.4). Traceability can be an effective tool to help prevent
4195   key compromises and reduce the impacts of compromises when they are detected. Providing
4196   traceability for a system requires identifying the entities accessing the system and employing
4197   access-control mechanisms (see Sec. 8.1).
4198   Although it is preferred that no humans be able to view keys, as a minimum, any access to
4199   plaintext cryptographic keys shall be traceable to the entity accessing them, regardless of
4200   whether the entity is a human, device, application, or process. Access to ciphertext keys and



                                                        131


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

4201   key shares should also be traceable to the accessing entities. For example, a sophisticated
4202   traceability system might be able to determine each entity that had control of any given key
4203   over its entire lifespan. This would include the entity that generated the key, the entity that
4204   used the key to cryptographically protect data, any other entity known to have accessed the
4205   key, and the entity that was responsible for destroying the key when it was no longer needed.
4206   Even though these other entities may never have actually accessed the key in its plaintext
4207   form, any actions they performed on or with the key should be traceable to them.
4208   Traceability provides three significant advantages:
4209       1. It helps determine when a compromise could have occurred and what entities could
4210          have been involved.
4211       2. It tends to discourge compromise attempts because individuals with access to the key
4212          know that their access to the key is known, and the developers of devices,
4213          applications, and processes that access the key know that access will be traced to
4214          these entities.
4215       3. When recovering from a detected key compromise, it is very useful to know where
4216          the key was used and what data or other keys were protected by the compromised
4217          key.
4218   Certain principles are useful when enforcing the traceability of cryptographic key use,
4219   although these principles might not be applicable to all systems or all key types. The principles
4220   include:
4221       •    Uniquely identifying keys
4222       •    Identifying other keys that are protected by a symmetric or private key
4223       •    Logging (i.e., recording) any activity related to keys or the associated metadata,
4224            including their generation, access, modification, revocation, destruction, or any other
4225            access to them. Section 7.2.4 in [SP 800-152] lists appropriate information that shall
4226            be recorded for key management events (e.g., key generation and destruction).

4227   8.4. Audit
4228   The auditing of activities associated with key management is required. Three types of audits
4229   should be performed on key-management systems:
4230       1. Initial and periodic compliance audits should be conducted to determine that a key-
4231          management system is prepared to operate or continues to operate in compliance
4232          with its key management policy and practice requirements. This includes 1) an
4233          examination of the security plan and the procedures that are developed to support
4234          the plan to determine whether they support the policy (see [SP 800-57p2]) and 2)
4235          confirmation that roles and responsibilities are defined, all participants have been
4236          educated and understand their roles, systems are in place to maintain an accurate
4237          inventory of all keys, the processes used are appropriate for the applications and risks,



                                                        132


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)              Recommendation for Key Management
       December 2025                                                                  Part 1 — General

4238            access controls are properly implemented, and access is removed when personnel are
4239            reassigned.
4240       2. The protective mechanisms employed (e.g., access control mechanisms) should be
4241          periodically reassessed with respect to the level of security that they provide and are
4242          expected to provide in the future and whether the mechanisms correctly and
4243          effectively support the appropriate policies. New technological developments and
4244          attacks should be taken into consideration.
4245       3. On a more frequent basis, the actions of the entities that use, operate, and maintain
4246          the system should be reviewed to verify that they continue to follow established
4247          security procedures and have accessed only those keys and metadata for which they
4248          are authorized. This is normally accomplished by examining the logs created to record
4249          security-relevant events. Strong cryptographic systems can be compromised by lax
4250          and inappropriate actions. Highly unusual events should be noted and reviewed as
4251          possible indicators of attempted attacks on the system.
4252   Audit reports shall be provided as specified in the key-management policy (e.g., to a System
4253   Authority).

4254   8.5. Key-Management System Survivability
4255   A failure in a key-management system or its environment could hamper or prevent access to
4256   an organization’s stored information. Disaster recovery requires having procedures and a
4257   sufficient backup capability to recover from facility damage, utility service outages,
4258   communication and computation outages, hardware and software failures, and other failures
4259   that result in the corruption or loss of the stored key information or the key-management
4260   system itself.
4261   [OMB 11-01] notes that encryption is an important tool for protecting the confidentiality of
4262   disclosure-sensitive information that is entrusted to an agency’s care but that the encryption
4263   of agency data also presents risks to the availability of information needed for mission
4264   performance. Agencies are reminded of the need to protect the continuity of their
4265   information technology operations and agency services when implementing encryption. The
4266   guidance specifically notes that without access to the cryptographic keys that are needed to
4267   decrypt information, organizations risk losing their access to that information. The guidance
4268   particularly stresses that agencies must address information availability and assurance
4269   requirements through appropriate data-recovery mechanisms, such as cryptographic key
4270   recovery. Consequently, it is prudent to retain backed up or archived copies of the keys
4271   necessary to decrypt stored enciphered information, including master keys, key-wrapping
4272   keys, and the related keying material necessary to decrypt encrypted information until there
4273   is no longer any requirement for access to the underlying plaintext information (see Sec.
4274   8.5.2, Table 10, and Table 11).




                                                        133


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

4275   8.5.1. Backed Up and Archived Key
4276   [OMB 11-01] focuses on the need to recover decryption keys to decrypt encrypted
4277   information (see Sec. 8.5). However, as the tables in Sec. 7.2.2.1 show, there are other
4278   operational keys that organizations may need to backup or archive (e.g., public signature-
4279   verification keys and authorization keys). Backed up or archived copies of keying material
4280   shall be stored in accordance with the provisions of Sec. 5 to protect the confidentiality of
4281   encrypted information and the integrity of source authentication, identity authentication,
4282   integrity authentication, and authorization processes.

4283   8.5.2. Key Recovery
4284   Key recovery is the process of retrieving or reconstructing a key from key backups or archives
4285   in order to process cryptographically protected information (e.g., to decrypt encrypted
4286   information or verify a signature on signed information). There are several issues associated
4287   with key recovery, including:
4288       1. Which key information, if any, needs to be backed up or archived for later recovery?
4289       2. Where will backed up or archived key information be stored?
4290       3. When will archiving be done (e.g., during key activation or at the end of a key’s
4291          cryptoperiod)?
4292       4. Who will be responsible for protecting the backed up or archived key information?
4293       5. What procedures need to be in place for storing and recovering the key information?
4294       6. Who can request a recovery of the key information and under what conditions?
4295       7. Who will be notified when a key recovery has taken place and under what conditions?
4296       8. What audit or accounting functions need to be performed to ensure that the key
4297          information was only provided to authorized entities?
4298   Key recovery itself does not result in the deletion of key information from backups or
4299   archives.
4300   The permissible use of a key after recovery may depend on its cryptoperiod (see Sec. 4.3.4
4301   and 4.3.5). Whether a key should be recovered and used and from where it should be
4302   recovered depends on several factors, including its cryptoperiod, class (i.e., symmetric or
4303   asymmetric), use or purpose, and whether it has been compromised or suspected of being
4304   compromised.
4305   When a key has been backed up or archived, keys may be recovered and used as follows:
4306       1. If the key is not known to be or suspected of being compromised:
4307                •    Secret (symmetric) key:
4308                     The recovered key may be used for applying protection (e.g., for encryption)
4309                     only if the key’s originator-usage period has not been exceeded. The



                                                        134


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                        Recommendation for Key Management
       December 2025                                                                                            Part 1 — General

4310                        recovered key should be revoked as soon as possible, thus ending its
4311                        originator-usage period. If continued functionality is needed after revocation
4312                        for applying protection, a new key shall be generated to replace the recovered
4313                        key for applying cryptographic protection.
4314                        The recovered key may be used to process protected data (e.g., used for
4315                        decrypting ciphertext data) if the recipient-usage period of the key has not
4316                        been exceeded or the key was recovered from archive storage.
4317                   •    Private key of an asymmetric key pair:
4318                        A recovered private signature key may be used for signature generation if the
4319                        key’s cryptoperiod has not been exceeded. Recall that backing up a private
4320                        signature key is discouraged in most cases (see Sec. 7.2.2.1), and archiving a
4321                        private signature key is disallowed (see Sec. 7.3.1). A recovered private
4322                        signature key should be revoked 47 as soon as possible. If continued
4323                        functionality is needed after revocation, a new signature key pair shall be
4324                        generated.
4325                        A recovered private key-transport key may be used to decrypt keys if the
4326                        cryptoperiod has not been exceeded.
4327                        A recovered private key-agreement key may be used to establish new keys if
4328                        the cryptoperiod has not been exceeded. A recovered private key-agreement
4329                        key should be revoked 48 as soon as possible. If continued functionality is
4330                        needed after revocation, a new key pair shall be generated.
4331                        A recovered private decapsulation key may be used to decapsulate keys if the
4332                        cryptoperiod has not been exceeded.
4333                   •    Public key of an asymmetric key pair:
4334                        Recovered public signature-verification keys, public key-transport keys, and
4335                        public encapsulation keys may be used for their assigned purposes (i.e.,
4336                        signature verification, encryption, or encapsulation, respectively) if the
4337                        cryptoperiod has not been exceeded.
4338                        A recovered public key-agreement key may be used if the cryptoperiod of the
4339                        key has not been exceeded.
4340        2. If the key information has been compromised, the recovered key shall be revoked
4341           (see Sec. 8.5.4). The recovered key should only be used if the level of risk is
4342           acceptable, as determined by the user and/or the user’s organization.
4343                   •    Secret symmetric key:



       47
          If the corresponding public key has been included in a public-key certificate, the revocation of the key pair can be accomplished by
       revoking the public key in the certificate.
       48 If the corresponding public key has been included in a public-key certificate, the revocation of the key pair can be accomplished by

       revoking the public key in the certificate.



                                                                        135


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

4344                     The recovered key shall not be used to apply protection (e.g., encryption). If
4345                     continued functionality is needed to apply protection, a new key shall be
4346                     generated.
4347                     The recovered key may be used to process protected data (e.g., used for
4348                     decryption).
4349                •    Private key of an asymmetric key pair:
4350                     A recovered private signature-key shall not be used.
4351                     A recovered private key-transport key or private decapsulation key may be
4352                     used for its assigned purpose (i.e., key decryption or decapsulation,
4353                     respectively) if the risk of doing so is acceptable.
4354                     A recovered private key-agreement key shall not be used to establish new
4355                     keys, although it may be used to reconstruct already established keys.
4356                •    Public key of an asymmetric key pair:
4357                     A recovered public signature-verification key may be used for signature
4358                     verification.
4359                     A recovered public key-transport key or public encapsulation key shall not be
4360                     used.
4361                     A recovered public key-agreement key shall not be used to establish new keys,
4362                     although it may be used to reconstruct already established keys.

4363   8.5.3. System Redundancy/Contingency Planning
4364   Cryptography is a useful tool for preventing unauthorized access to data and/or resources,
4365   but when the mechanism fails, the use of cryptography can prevent access by valid entities
4366   to critical information and processes. For example, loss or corruption of the only copy of a
4367   decryption key can deny access to encrypted information. The continuity of an organization’s
4368   operations can depend heavily on contingency planning for key-management systems with a
4369   redundancy of critical logical processes and elements, including key management and
4370   cryptographic keys.

4371   8.5.3.1. General Principles
4372   Planning for recovery from system failures is an essential management function.
4373   Interruptions of critical infrastructure services should be anticipated, and a plan for
4374   maintaining the continuity of operations in support of an organization’s primary mission
4375   requirements shall be in place. With respect to key management, the following situations are
4376   typical of those for which planning is necessary:
4377       •    Lost key cards or tokens
4378       •    Forgotten authenticators (e.g., tokens or passwords) that control access to keys


                                                        136


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
       December 2025                                                                     Part 1 — General

4379       •    The failure of key-input devices (e.g., readers, PIV cards)
4380       •    The loss or corruption of the memory media on which keys and/or certificates are
4381            stored
4382       •    The compromise of keys
4383       •    Certificate termination without replacement certificates being installed
4384       •    The corruption of CRLs or CKLs
4385       •    Hardware failure of key or certificate generation, registration, and/or distribution
4386            systems, subsystems, or components
4387       •    Power loss that requires the re-initialization of key or certificate generation,
4388            registration, key-establishment systems, subsystems, or components
4389       •    The corruption of the memory media necessary for key or certificate generation,
4390            registration, key-establishment systems, subsystems, or components
4391       •    The loss or corruption of key or certificate distribution records and/or audit logs
4392       •    The loss or corruption of the association of keys to the key owners
4393       •    The unavailability of older software or hardware that is needed to access key
4394            information or process protected information
4395   While recovery discussions most commonly focus on the recovery of encrypted data and the
4396   restoration of encrypted communication capabilities, planning should also address 1) the
4397   restoration of access when cryptography is used in access control mechanisms without
4398   creating a temporary loss of access protections, 2) the restoration of critical processes when
4399   cryptography is used in authorization mechanisms without creating a temporary loss of
4400   authorization restrictions, and 3) the maintenance/restoration of integrity protection in
4401   digital signature and message authentication applications.
4402   Contingency planning should include 1) providing a means and assigning responsibilities for
4403   rapidly recognizing and reporting critical failures; 2) assigning responsibilities and providing
4404   resources for bypassing or replacing failed systems, subsystems, and components; and 3)
4405   establishing detailed bypass and/or recovery procedures.
4406   Contingency planning includes a full range of integrated logistical support functions. Spare
4407   parts (e.g., copies of critical devices, software programs, manuals, data files) should be
4408   available (either acquired or arranged for) and pre-positioned or delivery-staged. Emergency
4409   maintenance, replacement, and/or bypass instructions should be prepared and disseminated
4410   to designated individuals and at an accessible and advertised access point. Designated
4411   individuals should be trained in their assigned recovery procedures, and all personnel should
4412   be trained in reporting and recovery procedures.




                                                        137


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                    Recommendation for Key Management
       December 2025                                                                        Part 1 — General

4413   8.5.3.2. Cryptography and Key-Management-Specific Recovery Issues
4414   A key recovery capability generally involves some redundancy or multiple copies of key
4415   information. If one copy of critical key information is lost or corrupted, another copy usually
4416   needs to be available to recover data and/or restore capabilities. However, the more copies
4417   of a key that exist and are distributed to different locations, the more susceptible the key is
4418   to compromise by penetration of the storage location or subversion of the custodian (e.g.,
4419   user, service agent, or key production/distribution facility). In this sense, key confidentiality
4420   requirements conflict with continuity of operations requirements. Special care needs to be
4421   taken to safeguard all copies of key information, especially information about symmetric keys
4422   and private (asymmetric) keys. More details regarding contingency plans and planning
4423   requirements are provided in [SP 800-57p2].

4424   8.5.4. Compromise Recovery
4425   When a secret or private key that is used to protect sensitive information or critical processes
4426   is disclosed to unauthorized entities, all of the information and/or processes protected by
4427   that key become immediately subject to disclosure, modification, subversion, and/or denial
4428   of service. All compromised keys shall be revoked; all affected keys shall be replaced, if
4429   needed; and, where sensitive or critical information or processes are affected, an immediate
4430   damage assessment should be conducted. Measures necessary to mitigate the consequences
4431   of a suspected unauthorized access to protected data or processes and to reduce the
4432   probability or frequency of future compromises should be undertaken.
4433   Where secret (symmetric) keys or private (asymmetric) keys are used to protect only a single
4434   entity’s local information or communications between a single pair of entities, the
4435   compromise recovery process can be relatively simple and inexpensive. Damage assessment
4436   and mitigation measures are often local matters.
4437   However, when a key is shared by or affects a large number of entities, damage can be
4438   widespread, and recovery is both complex and expensive. Some examples of keys whose
4439   compromise might be particularly difficult or expensive to recover from include the following:
4440       •    A CA’s private signature key, especially if it is used to sign a root certificate in a public-
4441            key infrastructure
4442       •    A symmetric key-wrapping key shared by a large number of entities
4443       •    A key-derivation key/master key used in the derivation of keys by a large number of
4444            entities
4445       •    A symmetric data-encryption key used to encrypt data in a large, distributed database
4446       •    A symmetric key shared by a large number of communications network participants
4447       •    A key used to protect a large number of stored keys
4448   In all of these cases, a large number of key owners and relying parties (e.g., all parties
4449   authorized to use the secret key of a symmetric-key algorithm or the public key of an


                                                         138


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

4450   asymmetric-key algorithm) need to be immediately notified of the compromise. The inclusion
4451   of the key identifier on a CKL or the certificate serial number on a CRL to be published at a
4452   later date might not be sufficient. This means that a list of the most likely affected entities
4453   may need to be maintained, and a means for communicating the news of a compromise will
4454   be required. Particularly in the case of a compromised symmetric key, the news of a
4455   compromise and the replacement of keys should be sent only to the affected entities so as
4456   not to encourage others to exploit the situation.
4457   A secure path for replacing compromised keys is required. To permit the rapid restoration of
4458   service, an automated (e.g., over-the-air or network-based) replacement path is preferred
4459   (see Sec. 7.2.3). In some cases, however, there may be no practical alternative to manual
4460   distribution (e.g., the compromise of a root CA’s private key). A contingency distribution of
4461   alternative keys may help restore service rapidly in some circumstances (e.g., the
4462   compromise of a widely held symmetric key), but the possibility of a simultaneous
4463   compromise of operational and contingency keys would need to be considered.
4464   Damage assessment can be extraordinarily complex, particularly for the compromise and
4465   replacement of CA private keys, widely used key-transport or encapsulation keys, and keys
4466   that are used by many users of large, distributed databases.




                                                        139


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

4467   References
4468   [CSWP15]            Barker W, Polk W, Souppaya M (2021) Getting Ready for Post-Quantum
4469                       Cryptography: Exploring Challenges Associated with Adopting and Using
4470                       Post-Quantum Cryptographic Algorithms. (National Institute of Standards
4471                       and Technology, Gaithersburg, MD), NIST Cybersecurity White Paper (CSWP)
4472                       CSWP 15. https://doi.org/10.6028/NIST.CSWP.15
4473   [DiCrescenzo]       Di Crescenzo G, Ferguson N, Impagliazzo R, Jakobsson M (1999) How to
4474                       forget a secret. STACS 99: 16th Annual Symposium on Theoretical Aspects of
4475                       Computer Science (Springer, Trier, Germany), pp 500-509.
4476                       https://doi.org/10.1007/3-540-49116-3_47
4477   [Grassl]            Grassl M, Langenberg B, Roetteler M, and Steinwandt R, (2016) Applying
4478                       Grover’s algorithm to AES: quantum resource estimates, in Takagi T ed. Post-
4479                       Quantum Cryptography, Lect. Notes in Comput. Sci. vol. 9606, Springer, pp.
4480                       9– 43. Available at https://arxiv.org/abs/1512.04965
4481   [FIPS 140-3]        National Institute of Standards and Technology (2019) Security
4482                       Requirements for Cryptographic Modules. (U.S. Department of Commerce,
4483                       Washington, D.C.), Federal Information Processing Standards Publication
4484                       (FIPS) FIPS 140-3. https://doi.org/10.6028/NIST.FIPS.140-3
4485   [FIPS 180]          National Institute of Standards and Technology (2015) Secure Hash Standard
4486                       (SHS). (U.S. Department of Commerce, Washington, D.C.), Federal
4487                       Information Processing Standards Publication (FIPS) FIPS 180-4.
4488                       https://doi.org/10.6028/NIST.FIPS.180-4
4489   [FIPS 186-4]        National Institute of Standards and Technology (2013) Digital Signature
4490                       Standard (DSS). (U.S. Department of Commerce, Washington, D.C.), Draft
4491                       Federal Information Processing Standards Publication (FIPS) FIPS 186-4.
4492                       https://doi.org/10.6028/NIST.FIPS.186-4
4493   [FIPS 186-5]        National Institute of Standards and Technology (2023) Digital Signature
4494                       Standard (DSS). (U.S. Department of Commerce, Washington, D.C.), Draft
4495                       Federal Information Processing Standards Publication (FIPS) FIPS 186-5.
4496                       https://doi.org/10.6028/NIST.FIPS.186-5
4497   [FIPS 197]          National Institute of Standards and Technology (2023) Advanced Encryption
4498                       Standard (AES). (U.S. Department of Commerce, Washington, DC), Federal
4499                       Information Processing Standards Publication (FIPS) FIPS 197.
4500                       https://doi.org/10.6028/NIST.FIPS.197-upd1
4501   [FIPS 199]          National Institute of Standards and Technology (2004) Standards for Security
4502                       Categorization of Federal Information and Information Systems. (U.S.
4503                       Department of Commerce, Washington, D.C.), Federal Information
4504                       Processing       Standards       Publication      (FIPS)     FIPS      199.
4505                       https://doi.org/10.6028/NIST.FIPS.199



                                                        140


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
       December 2025                                                                     Part 1 — General

4506   [FIPS 201-3]        National Institute of Standards and Technology (2022) Personal Identity
4507                       Verification (PIV) of Federal Employees and Contractors, (U.S. Department
4508                       of Commerce, Washington, D.C.), Federal Information Processing Standards
4509                       Publication (FIPS) FIPS 201-3. https://doi.org/10.6028/NIST.FIPS.201-3
4510   [FIPS 202]          National Institute of Standards and Technology (2015) SHA-3 Standard:
4511                       Permutation-Based Hash and Extendable-Output Functions. (U.S.
4512                       Department of Commerce, Washington, D.C.), Federal Information
4513                       Processing       Standards      Publication   (FIPS)    FIPS     202.
4514                       https://doi.org/10.6028/NIST.FIPS.202
4515   [FIPS 203]          National     Institute    of  Standards     and     Technology  (2024).
4516                       Module-Lattice-Based Key-Encapsulation Mechanism Standard. (U.S.
4517                       Department of Commerce, Washington, D.C.), Federal Information
4518                       Processing        Standards     Publication     (FIPS)     FIPS    203.
4519                       https://doi.org/10.6028/NIST.FIPS.203
4520   [FIPS 204]          National     Institute    of     Standards     and    Technology   (2024.
4521                       Module-Lattice-Based Digital Signature Standard. (U.S. Department of
4522                       Commerce, Washington, D.C.), Federal Information Processing Standards
4523                       Publication (FIPS) FIPS 204. https://doi.org/10.6028/NIST.FIPS.204
4524   [FIPS 205]          National     Institute    of     Standards     and    Technology   (2024)
4525                       Stateless Hash-Based Digital Signature Standard. (U.S. Department of
4526                       Commerce, Washington, D.C.), Federal Information Processing Standards
4527                       Publication (FIPS) FIPS 205. https://doi.org/10.6028/NIST.FIPS.205
4528   [FPKI-KRP]          Federal Public Key Infrastructure Policy Authority (2017) Federal Public Key
4529                       Infrastructure Key Recovery Policy, version 1.0. Available at
4530                       https://www.idmanagement.gov/docs/archived/fpki-key-
4531                       recovery_20240205.pdf
4532   [IGD_B]             National Institute of Standards and Technology, Canadian Centre for Cyber
4533                       Security (2024) Implementation Guidance for FIPS 140-3 and the
4534                       Cryptographic Module Validation Program, [Amended]. Available at
4535                       https://csrc.nist.gov/csrc/media/Projects/cryptographic-module-
4536                       validation-program/documents/fips%20140-3/FIPS%20140-3%20IG.pdf
4537   [ISO/IEC 19790] ISO/IEC 19790, Information technology — Security techniques — Security
4538                   requirements for cryptographic modules 2025. Available at
4539                   https://www.iso.org/standard/82423.html
4540   [ITLBulletin]       Burr WE, Hash JS (2002) Techniques for System and Data Recovery. (National
4541                       Institute of Standards and Technology, Gaithersburg, MD), ITL Bulletin, April
4542                       2002. Available at https://csrc.nist.gov/files/pubs/shared/itlb/itlbul2002-
4543                       04.pdf
4544   [Jones]             Jones NC, Van Meter R, Fowler AG, McMahon PL, Kim J, Ladd TD, Yamamoto
4545                       Y (2012) Layered Architecture for Quantum Computing. American Physical


                                                        141


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                  Recommendation for Key Management
       December 2025                                                                      Part 1 — General

4546                       Society,     Phys.      Rev.     X     2,    031007.     Available          at
4547                       https://journals.aps.org/prx/abstract/10.1103/PhysRevX.2.031007
4548   [Mariantoni]        Mariantoni M, (2014) Building a Superconducting Quantum Computer,
4549                       Invited Talk PQCrypto 2014, Waterloo, Canada. Available at
4550                       https://youtu.be/wWHAs--HA1c
4551   [NIST IR 7977]       Cryptographic Technology Group (2016) NIST Cryptographic Standards and
4552                        Guidelines Development Process. (National Institute of Standards and
4553                        Technology, Gaithersburg, MD), NIST Internal Report (IR) NIST IR 7977.
4554                        https://doi.org/10.6028/NIST.IR.7977
4555   [NIST PQC eval] National Institute of Standards and Technology (2016) NIST Submission
4556                   Requirements and Evaluation Criteria for the Post-Quantum Cryptography
4557                   Standardization             Process.             Available          at
4558                   https://csrc.nist.gov/CSRC/media/Projects/Post-Quantum-
4559                   Cryptography/documents/call-for-proposals-final-dec-2016.pdf
4560   [OMB 11-01]         Office of Management and Budget (2001) OMB Guidance to Federal
4561                       Agencies on Data Availability and Encryption. (National Institute of
4562                       Standards and Technology, Gaithersburg, MD), [November 26, 2001].
4563                       Available   at   https://csrc.nist.gov/csrc/media/projects/block-cipher-
4564                       techniques/documents/ombencryption-guidance.pdf
4565   [RFC 2560]          Santesson S, Myers M, Ankney R, Malpani A, Galperin S, Adams C (2013)
4566                       X.509 Internet Public Key Infrastructure, Online Certificate Status Protocol –
4567                       OCSP. (Internet Engineering Task Force (IETF) Network Working Group), IETF
4568                       Request      for      Comments        (RFC)      6960.        Available     at
4569                       https://datatracker.ietf.org/doc/html/rfc6960
4570   [RFC 3647]          Chokhani S, Ford W, Sabett R, Merrill C, Wu S (2003) Internet X.509 Public
4571                       Key Infrastructure Certificate Policy and Certification Practices Framework
4572                       (Internet Engineering Task Force (IETF) Network Working Group), IETF
4573                       Request for Comments (RFC) 3647. https://doi.org/10.17487/RFC3647
4574   [RFC 8032]          Josefsson S, Liusvaara I (2017) Edwards-Curve Digital Signature Algorithm
4575                       (EdDSA). (Internet Research Task Force (IRTF)), IRTF Request for Comments
4576                       (RFC) 8032. https://doi.org/10.17487/RFC8032
4577   [SP 800-37]         Joint Task Force (2018) Risk Management Framework for Information
4578                       Systems and Organizations: A System Life Cycle Approach for Security and
4579                       Privacy. (National Institute of Standards and Technology, Gaithersburg, MD),
4580                       NIST       Special      Publication      (SP)     NIST     SP      800-37r2.
4581                       https://doi.org/10.6028/NIST.SP.800-37r2
4582   [SP 800-38]         Dworkin M (2001) Recommendation for Block Cipher Modes of Operation.
4583                       (National Institute of Standards and Technology, Gaithersburg, MD), NIST
4584                       Special    Publication     (SP)    NIST    SP    800-38.   Available  at
4585                       https://csrc.nist.gov/projects/block-cipher-techniques/bcm/current-modes


                                                        142


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

4586   [SP 800-38A]        Dworkin MJ (2001) Recommendation for Block Cipher Modes of Operation:
4587                       Methods and Techniques. (National Institute of Standards and Technology,
4588                       Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-38A.
4589                       https://doi.org/10.6028/NIST.SP.800-38A
4590   [SP 800-38B]        Dworkin MJ (2005) Recommendation for Block Cipher Modes of Operation:
4591                       the CMAC Mode for Authentication. (National Institute of Standards and
4592                       Technology, Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-
4593                       38B,     Includes     updates     as    of     October       6,     2016.
4594                       https://doi.org/10.6028/NIST.SP.800-38B
4595   [SP 800-38C]        Dworkin MJ (2004) Recommendation for Block Cipher Modes of Operation:
4596                       the CCM Mode for Authentication and Confidentiality. (National Institute of
4597                       Standards and Technology, Gaithersburg, MD), NIST Special Publication (SP)
4598                       NIST SP 800-38C, Includes updates as of July 20, 2007.
4599                       https://doi.org/10.6028/NIST.SP.800-38C
4600   [SP 800-38D]        Dworkin MJ (2007) Recommendation for Block Cipher Modes of Operation:
4601                       Galois/Counter Mode (GCM) and GMAC. (National Institute of Standards and
4602                       Technology, Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-
4603                       38D. https://doi.org/10.6028/NIST.SP.800-38D
4604   [SP 800-38F]        Dworkin MJ (2012) Recommendation for Block Cipher Modes of Operation:
4605                       Methods for Key Wrapping. (National Institute of Standards and Technology,
4606                       Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-38F.
4607                       https://doi.org/10.6028/NIST.SP.800-38F
4608   [SP 800-52]         Polk T, McKay KA, Chokhani S (2019) Guidelines for the Selection,
4609                       Configuration, and Use of Transport Layer Security (TLS) Implementations.
4610                       (National Institute of Standards and Technology, Gaithersburg, MD), NIST
4611                       Special        Publication       (SP)      NIST        SP      800-52r2.
4612                       https://doi.org/10.6028/NIST.SP.800-52r2
4613   [SP 800-56A]        Barker EB, Chen L, Roginsky A, Vassilev A, Davis R (2018) Recommendation
4614                       for Pair-Wise Key-Establishment Schemes Using Discrete Logarithm
4615                       Cryptography. (National Institute of Standards and Technology,
4616                       Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-56Ar3.
4617                       https://doi.org/10.6028/NIST.SP.800-56Ar3
4618   [SP 800-56B]        Barker EB, Chen L, Roginsky A, Vassilev A, Davis R, Simon S (2019)
4619                       Recommendation for Pair-Wise Key-Establishment Using Integer
4620                       Factorization Cryptography. (National Institute of Standards and
4621                       Technology, Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-
4622                       56Br2. https://doi.org/10.6028/NIST.SP.800-56Br2
4623   [SP 800-56C]        Barker EB, Chen L, Davis R (2018) Recommendation for Key-Derivation
4624                       Methods in Key-Establishment Schemes. (National Institute of Standards




                                                        143


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
       December 2025                                                                     Part 1 — General

4625                       and Technology, Gaithersburg, MD), NIST Special Publication (SP) NIST SP
4626                       800-56Cr1. Withdrawn. https://doi.org/10.6028/NIST.SP.800-56Cr1
4627   [SP 800-57p2]       Barker EB, Barker WC (2019) Recommendation for Key Management: Part 2
4628                       – Best Practices for Key Management Organizations. (National Institute of
4629                       Standards and Technology, Gaithersburg, MD), NIST Special Publication (SP)
4630                       NIST SP 800-57pt2r1. https://doi.org/10.6028/NIST.SP.800-57pt2r1
4631   [SP 800-57p3]       Barker EB, Dang QH (2015) Recommendation for Key Management, Part 3:
4632                       Application-Specific Key Management Guidance. (National Institute of
4633                       Standards and Technology, Gaithersburg, MD), NIST Special Publication (SP)
4634                       NIST SP 800-57pt3r1. https://doi.org/10.6028/NIST.SP.800-57pt3r1
4635   [SP 800-63]         Temoshok D, Madruga DP, Choong YY, Galluzzo R, Gupta S, LaSalle C,
4636                       Lefkovitz N, Regenscheid A (2024) Digital Identity Guidelines. (National
4637                       Institute of Standards and Technology, Gaithersburg, MD), NIST Special
4638                       Publication (SP) NIST SP 800-63-4 2pd. https://doi.org/10.6028/NIST.SP.800-
4639                       63-4.2pd
4640   [SP 800-63A]        Temoshok D, Abruzzi C, Choong YY, Fenton JL, Galluzzo R, LaSalle C, Lefkovitz
4641                       NB, Regenscheid A (2024) Digital Identity Guidelines: Enrollment and
4642                       Identity Proofing. (National Institute of Standards and Technology,
4643                       Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-63A-4 2pd.
4644                       https://doi.org/10.6028/NIST.SP.800-63A-4.2pd
4645   [SP 800-67]         Barker EB, Mouha N (2017) Recommendation for the Triple Data Encryption
4646                       Algorithm (TDEA) Block Cipher. (National Institute of Standards and
4647                       Technology, Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-
4648                       67r2. Withdrawn. https://doi.org/10.6028/NIST.SP.800-67r2
4649   [SP 800-88]         Chandramouli R,,Hibbard E (2025) Guidelines for Media Sanitization.
4650                       (National Institute of Standards and Technology, Gaithersburg, MD), NIST
4651                       Special        Publication       (SP)      NIST        SP       800-88r2.
4652                       https://csrc.nist.gov/pubs/sp/800/88/r2/final#:~:text=https%3A//doi.org/1
4653                       0.6028/NIST.SP.800%2D88r2
4654   [SP 800-89]         Barker EB (2006) Recommendation for Obtaining Assurances for Digital
4655                       Signature Applications. (National Institute of Standards and Technology,
4656                       Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-89.
4657                       https://doi.org/10.6028/NIST.SP.800-89
4658   [SP 800-90A]        Barker EB, Kelsey JM (2015) Recommendation for Random Number
4659                       Generation Using Deterministic Random Bit Generators. (National Institute
4660                       of Standards and Technology, Gaithersburg, MD), NIST Special Publication
4661                       (SP) NIST SP 800-90Ar1. https://doi.org/10.6028/NIST.SP.800-90Ar1
4662   [SP 800-90B]        Sönmez Turan M, Barker EB, Kelsey JM, McKay KA, Baish ML, Boyle M (2018)
4663                       Recommendation for the Entropy Sources Used for Random Bit Generation.
4664                       (National Institute of Standards and Technology, Gaithersburg, MD), NIST



                                                        144


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

4665                       Special        Publication      (SP)        NIST        SP         800-90B.
4666                       https://doi.org/10.6028/NIST.SP.800-90B
4667   [SP 800-90C]        Barker EB, Kelsey JM, McKay K, Roginsky A, Sönmez Turan M (2025)
4668                       Recommendation for Random Bit Generator (RBG) Constructions. (National
4669                       Institute of Standards and Technology, Gaithersburg, MD), NIST Special
4670                       Publication (SP) NIST SP 800-90C.
4671                       https://doi.org/10.6028/NIST.SP.800-90C
4672   [SP 800-108]        Chen L (2024) Recommendation for Key Derivation Using Pseudorandom
4673                       Functions (Revised). (National Institute of Standards and Technology,
4674                       Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-108r1.
4675                       https://doi.org/10.6028/NIST.SP.800-108r1-upd1
4676   [SP 800-130]        Barker EB, Smid ME, Branstad DK, Chokhani S (2013) A Framework for
4677                       Designing Cryptographic Key Management Systems. (National Institute of
4678                       Standards and Technology, Gaithersburg, MD), NIST Special Publication (SP)
4679                       NIST SP 800-130. https://doi.org/10.6028/NIST.SP.800-130
4680   [SP 800-131A]       Barker EB, Roginsky A (2024) Transitioning the Use of Cryptographic
4681                       Algorithms and Key Lengths. (National Institute of Standards and
4682                       Technology, Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-
4683                       131Ar3 ipd. https://doi.org/10.6028/NIST.SP.800-131Ar3.ipd
4684   [SP 800-132]        Sönmez Turan M, Barker EB, Burr WE, Chen L (2010) Recommendation for
4685                       Password-Based Key Derivation: Part 1: Storage Applications. (National
4686                       Institute of Standards and Technology, Gaithersburg, MD), NIST Special
4687                       Publication (SP) NIST SP 800-132. https://doi.org/10.6028/NIST.SP.800-132
4688   [SP 800-133]        Barker EB, Roginsky AL (2019) Recommendation for Cryptographic Key
4689                       Generation. (National Institute of Standards and Technology, Gaithersburg,
4690                       MD), NIST Special Publication (SP) NIST SP 800-133r1. Withdrawn.
4691                       https://doi.org/10.6028/NIST.SP.800-133r1
4692   [SP 800-135]        Dang QH (2011) Recommendation for Existing Application-Specific Key
4693                       Derivation Functions. (National Institute of Standards and Technology,
4694                       Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-135r1.
4695                       https://doi.org/10.6028/NIST.SP.800-135r1
4696   [SP 800-140]        Schaeffer K (2020) FIPS 140-3 Derived Test Requirements (DTR): CMVP
4697                       Validation Authority Updates to ISO/IEC 24759. (National Institute of
4698                       Standards and Technology, Gaithersburg, MD), NIST Special Publication (SP)
4699                       NIST SP 800-140. https://doi.org/10.6028/NIST.SP.800-140
4700   [SP 800-152]        Barker EB, Branstad DK, Smid ME (2015) A Profile for U.S. Federal
4701                       Cryptographic Key Management Systems (CKMS). (National Institute of
4702                       Standards and Technology, Gaithersburg, MD), NIST Special Publication (SP)
4703                       NIST SP 800-152. https://doi.org/10.6028/NIST.SP.800-152




                                                        145


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

4704   [SP 800-175B]       Barker EB (2020) Guideline for Using Cryptographic Standards in the Federal
4705                       Government: Cryptographic Mechanisms. (National Institute of Standards
4706                       and Technology, Gaithersburg, MD), NIST Special Publication (SP) NIST SP
4707                       800-175Br1. https://doi.org/10.6028/NIST.SP.800-175Br1
4708   [SP 800-185]        Kelsey JM, Chang S-jH, Perlner RA (2016) SHA-3 Derived Functions: cSHAKE,
4709                       KMAC, TupleHash, and ParallelHash. (National Institute of Standards and
4710                       Technology, Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-
4711                       185. https://doi.org/10.6028/NIST.SP.800-185
4712   [SP 800-208]        Cooper D, Apon D, Dang Q, Davidson M, Dworkin M, Miller C (2020)
4713                       Recommendation for Stateful-Hash-Based Signature Schemes. (National
4714                       Institute of Standards and Technology, Gaithersburg, MD), NIST Special
4715                       Publication (SP) NIST SP 800-208. https://doi.org/10.6028/NIST.SP.800-208
4716   [SP 800-224]        Sönmez Turan M, Brandão (2024) Keyed-Hash Message Authentication Code
4717                       (HMAC): Specification of HMAC and Recommendations for Message
4718                       Authentication. (National Institute of Standards and Technology,
4719                       Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-224 ipd.
4720                       https://doi.org/10.6028/NIST.SP.800-224.ipd
4721   [SP 800-227]        Alagic G, Barker E, Chen L, Moody D, Robinson A Silberg H, Waller N (2025)
4722                       Recommendations for Key-Encapsulation Mechanisms. (National Institute
4723                       of Standards and Technology, Gaithersburg, MD), NIST Special Publication
4724                       (SP) NIST SP 800-227. https://doi.org/10.6028/NIST.SP.800-227
4725   [SP 800-232]        Sönmez Turan M, McKay K, Chang D, Kang J, Kelsey J (2025) Ascon-Based
4726                       Lightweight Cryptography Standards for Constrained Devices: Authenticated
4727                       Encryption, Hash, and Extendable Output Functions. (National Institute of
4728                       Standards and Technology, Gaithersburg, MD), NIST Special Publication (SP)
4729                       NIST SP 800-232. https://doi.org/10.6028/NIST.SP.800-232
4730   [X995]              American National Standard (ANS) X9.31-2022, Trusted Timestamp
4731                       Management and Security.
4732   [ISO/IEC18014] Timestamping:
4733                  ISO/IEC 18014-1:2008 Information technology – Security techniques – Time-
4734                  stamping services – Part 1: Framework
4735                  ISO/IEC 18014-2:2021 Information security – Time-stamping services – Part
4736                  2: Mechanisms producing independent tokens
4737                  ISO/IEC 18014-3:2009 Information technology – Security techniques – Time-
4738                  stamping services – Part 3: Mechanisms producing linked tokens
4739                  ISO/IEC 18014-4:2015 Information technology – Security techniques – Time-
4740                  stamping services – Part 4: Traceability of time sources
4741   [Zalka]        Zalka C (1999) Grover’s quantum searching algorithm is optimal, American
4742                  Physical Society, Phys. Rev. A 60, 2746. Available at
4743                  https://journals.aps.org/pra/abstract/10.1103/PhysRevA.60.2746



                                                        146


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                           Recommendation for Key Management
       December 2025                                                                                               Part 1 — General

4744   Appendix A. Cryptographic and Non-Cryptographic Integrity and Source Authentication
4745               Mechanisms
4746   Integrity and source authentication services are particularly important in protocols that
4747   include key management. When integrity or source-authentication services are discussed in
4748   this recommendation, the use of “strong” cryptographic integrity or source-authentication
4749   mechanisms is assumed. Secure communications and key management are typically provided
4750   using a communication protocol that offers certain services, such as integrity protection or a
4751   “reliable” transport service. 49 However, the integrity protection or reliable transport services
4752   of communication protocols are not necessarily adequate for cryptographic applications,
4753   particularly for key management, and there might be confusion about the meaning of terms
4754   such as “integrity.”
4755   All communication channels have some noise (i.e., unintentional errors inserted by the
4756   transmission media), and other factors (e.g., network congestion) can cause network
4757   packets 50 to be lost. Therefore, integrity protection and reliable transport services for
4758   communication protocols are designed to function over a channel with certain worst-case
4759   noise characteristics. entity that detects damaged packets (i.e., packets that contain bit
4760   errors) or lost packets Transmission bit errors are typically detected using 1) a non-
4761   cryptographic checksum 51 to detect transmission errors in a packet and 2) a packet counter
4762   to detect lost packets. A receiving may ask the sender to retransmit them. Non-cryptographic
4763   checksums are generally effective at detecting transmission noise. For example, the common
4764   CRC-32 checksum algorithm used in local-area network applications detects all error bursts
4765   with a span of less than 32 bits and detects longer random bursts with a 2−32 failure
4766   probability. However, the non-cryptographic CRC-32 checksum does not detect the swapping
4767   of 32-bit message words, and specific errors in particular message bits cause predictable
4768   changes in the CRC-32 checksum. The sophisticated attacker can take advantage of this to
4769   create altered messages that pass the CRC-32 integrity checks, even (in some cases) when
4770   the message is encrypted.
4771   Forward error-correcting codes are a subset of non-cryptographic checksums that can be
4772   used to correct a limited number of errors without retransmission. These codes may be used
4773   as checksums, depending on the application and noise properties of the communication
4774   channel.
4775   However, cryptographic integrity-authentication mechanisms (e.g., MACs, digital signatures)
4776   protect against an active, intelligent attacker who might attempt to disguise an attack as
4777   noise. Typically, the bits altered by the attacker are not random but target system properties
4778   and vulnerabilities. Cryptographic integrity-authentication mechanisms are effective at
4779   detecting random noise events, but they also detect more systematic deliberate attacks.

       49 This means transmitting information within a network using protocols that provide assurances that the information is received correctly.
       50 A network packet is a formatted unit of data that is used to send messages across a network. Messages may be divided into multiple

       packets for transmission efficiency.
       51
         A checksum is an algorithm that uses the bits in the transmission to create a checksum value, which is normally sent in the transmission.
       The receiver re-computes the checksum value using the bits in the received transmission and compares the received checksum value with
       the computed value to determine whether the transmission was correctly received. A non-cryptographic checksum algorithm uses a well-
       known algorithm without secret information (i.e., without a cryptographic key).



                                                                          147


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                          Recommendation for Key Management
       December 2025                                                                                              Part 1 — General

4780   Cryptographic hash functions, such as SHA-256, are designed to make every bit of the hash
4781   value a complex, nonlinear function of every bit of the message text and to make it
4782   impractical to find two messages that hash to the same value. On average, it is necessary to
4783   perform 2128 SHA-256 hash operations to find two messages that hash to the same value, and
4784   it is much harder to find another message whose SHA-256 hash is the same value as the hash
4785   of any given message. Cryptographic MAC algorithms employ hash functions or symmetric
4786   encryption algorithms and keys to authenticate the source of a message and to protect its
4787   integrity (i.e., detect errors). Digital signatures use public-key algorithms and hash functions
4788   or XOFs to provide both integrity and source-authentication services. Compared to non-
4789   cryptographic integrity or source-authentication mechanisms, these cryptographic services
4790   are usually computationally more expensive. This seems to be unavoidable, since
4791   cryptographic protections must also resist deliberate attacks by knowledgeable adversaries
4792   with substantial resources.
4793   Cryptographic and non-cryptographic integrity-authentication mechanisms may be used
4794   together. For example, in the TLS protocol (see [SP 800-52]), a client and a server can
4795   authenticate each other’s identity, establish a shared “master key,” and transfer encrypted
4796   payload data. Every step in the entire TLS protocol run is protected by cryptographic integrity
4797   and source-authentication mechanisms, and the payload is usually encrypted. Like most
4798   cryptographic protocols, TLS will (with a given probability) detect any attack or noise event
4799   that alters any part of the protocol run. However, TLS has no error-recovery protocol. If an
4800   error is detected, the protocol run is simply terminated. Starting a new TLS protocol run is
4801   quite expensive. Therefore, TLS requires a “reliable” transport service, typically the internet
4802   Transport Control Protocol (TCP), to handle and recover from ordinary network transmission
4803   errors. TLS will detect errors caused by an attack or noise event but has no mechanism to
4804   recover from them. TCP will generally detect such errors on a packet-by-packet basis and
4805   recover from them by retransmitting individual packets before delivering the data to TLS.
4806   Both TLS and TCP have integrity-authentication mechanisms, but a sophisticated attacker
4807   could easily fool the weaker non-cryptographic checksums of TCP. However, because of the
4808   cryptographic integrity-authentication mechanism provided in TLS, the attack is thwarted.
4809   There are some interactions between cryptographic and non-cryptographic integrity or error-
4810   correction mechanisms that users and protocol designers must consider. For example, many
4811   encryption modes expand ciphertext errors, and a single bit error in the ciphertext can change
4812   an entire block or more of the resulting plaintext. If forward error correction 52 (e.g., using a
4813   MAC algorithm, such as HMAC) is applied before encryption, and errors are inserted in the
4814   ciphertext during transmission, the error expansion during the decryption might
4815   “overwhelm” the error-correction mechanism, making the errors uncorrectable. Therefore,
4816   it is preferable to apply the forward error-correction mechanism after the encryption process
4817   (e.g., compute a MAC on the encrypted data rather than computing a MAC on the plaintext



       52
         Forward error correction is a technique used for controlling errors in data transmissions. The sender encodes the message in a redundant
       way by using an error-detection code. The redundancy allows the receiver to detect a limited number of errors that may occur anywhere in
       the message and, often, to correct these errors without re-transmission.




                                                                         148


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                            Recommendation for Key Management
       December 2025                                                                                                Part 1 — General

4818   data and then performing encryption). This will allow the receiving entity’s system to correct
4819   errors before the ciphertext is decrypted, resulting in “correct” plaintext.
4820   Interactions between cryptographic and non-cryptographic mechanisms can also result in
4821   security vulnerabilities. One classic way this occurs is with protocols that use stream ciphers53
4822   with non-cryptographic checksums (e.g., CRC-32) that are computed over the plaintext data
4823   and that acknowledge good packets. An attacker can copy the encrypted packet, selectively
4824   modify individual ciphertext bits, selectively change bits in the CRC, and then send the packet.
4825   Using the protocol’s acknowledgement mechanism, the attacker can determine when the
4826   CRC is correct and, therefore, determine certain bits of the underlying plaintext. At least one
4827   widely used wireless encryption protocol has been broken with such an attack.




       53 Stream ciphers encrypt and decrypt one element (e.g., bit or byte) at a time. There are no approved algorithms specifically designated as

       stream ciphers. However, some of the cryptographic modes defined in [SP 800-38] can be used with a symmetric block cipher algorithm,
       such as AES, to perform the function of a stream cipher.



                                                                          149


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

4828   Appendix B. Key Recovery

4829   B.1. General Discussion
4830   Federal agencies have a responsibility to protect the information contained in, processed by,
4831   and transmitted between their information technology systems. Cryptographic techniques
4832   are often used as part of this process. These techniques are used to provide confidentiality,
4833   integrity authentication, identity authentication, source authentication, non-repudiation,
4834   and/or access control. Policies shall be established to address the protection and continued
4835   accessibility of cryptographically protected information, and procedures shall be in place to
4836   ensure that the information remains viable during its lifetime. When cryptographic keying
4837   material is used to protect the information, this same keying material may need to be
4838   available to remove (e.g., decrypt) or verify those protections (e.g., verify the MAC).
4839   In many cases, the keying material used for cryptographic processes might not be readily
4840   available. This might be the case for several reasons, including:
4841       1. The cryptoperiod of the key has expired, and the keying material is no longer in
4842          operational storage.
4843       2. The keying material has been corrupted (e.g., the system has crashed, or a virus has
4844          modified the saved keying material in operational storage).
4845       3. The key’s owner is not available, and the owner’s organization needs to obtain the
4846          plaintext information.
4847   In order to have this keying material available when required, the keying material needs to
4848   be saved somewhere or be constructible (e.g., derivable) from other available keying
4849   material. The process of reacquiring the keying material is called key recovery. Key recovery
4850   is often used as one method of information recovery when the plaintext information needs
4851   to be recovered from encrypted information. However, keying material or other related
4852   information may need to be recovered for other reasons, such as the corruption of keying
4853   material in normal operational storage (e.g., for the verification of MACs for archived
4854   documents; see Sec. 7.2.1). Key recovery may also be appropriate for situations in which it is
4855   easier or faster to recover the keying material than it is to generate and distribute new keying
4856   material.
4857   However, there are applications that may not need to save the keying material for an
4858   extended time because of other procedures to recover an operational capability when the
4859   keying material or the information protected by the keying material becomes inaccessible.
4860   Applications of this type could include telecommunications, where the transmitted
4861   information could be resent or applications that could quickly derive or acquire new keying
4862   material for distribution.
4863   It is the responsibility of an organization to determine whether the recovery of keying
4864   material is required for their application. The decision for having a key-recovery capability
4865   should be made on a case-by-case basis, and this decision should be reflected in the Key-
4866   Management Policy and the Key-Management Practices Statement (see [SP 800-57p2]). If the


                                                        150


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

4867   decision is made to provide key recovery, the appropriate method of key recovery should be
4868   selected, designed, and implemented based on the type of keying material to be recovered.
4869   An appropriate entity needs to be selected to maintain the backup or archive database and
4870   manage the key-recovery process. If the decision is made to provide key recovery, all
4871   information associated with that key (e.g., metadata) shall also be recoverable.

4872   B.1.1. Recovery From Stored Keying Material
4873   The primary purpose of backing up or archiving keys and other key information is to be able
4874   to recover them when they are not otherwise available. For example, encrypted information
4875   cannot be transformed back into plaintext information if the decryption key is lost or
4876   modified; the integrity of data cannot be authenticated if the key used to verify the integrity
4877   of that data is not available. The key-recovery process retrieves the keying material from
4878   backup or archive storage and places it in a device, module, or other immediately accessible
4879   storage, often with the assistance of some human (see Sec. 7.3.1).

4880   B.1.2. Recovery by Reconstruction of Keying Material
4881   Some keying material may be recovered by reconstructing or re-deriving the keying material
4882   from other available keying material — that is, the “base” keying material (e.g., a key-
4883   derivation key for a key-derivation method or a seed). The base keying material shall be
4884   available in normal operational storage (see Sec. 7.2.1), backup storage (see Sec. 7.2.2.1), or
4885   archive storage (see Sec. 7.3.1).

4886   B.1.3. Conditions Under Which Keying Material Needs to be Recoverable
4887   The decision to back up or archive keying material for possible key recovery should be made
4888   on a case-by-case basis and should be based on the list provided in Sec. 7.2.2.2.
4889   When the key-recovery operation is requested by the key’s owner, the following actions shall
4890   be taken:
4891       1. If a lost key may have been compromised, then the key shall be replaced as soon as
4892          possible after recovery to limit the exposure of the recovered key and the data it
4893          protects (see Sec. 7.2.3.1). This could include reapplying the protection on the
4894          protected data using a new key.
4895       2. If the key becomes inaccessible or has been modified but compromise is not
4896          suspected, then the key may be recovered and used, as discussed in Sec. 8.5.2.
4897   Appendices B.2 through B.10 provide guidelines for determining whether a key-recovery
4898   capability is needed as well as other information that may be associated with the keys (e.g.,
4899   the metadata associated with the key).




                                                        151


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

4900   B.1.4. Key-Recovery Systems
4901   Key recovery is a broad term that may be applied to several different techniques for
4902   recovering a cryptographic key and information associated with that key (e.g., the key’s
4903   metadata). The information required to recover a key may be different for each application
4904   or each key-recovery technique. The term “key-recovery information” (KRI) refers to the
4905   aggregate of key information that is needed to recover or verify cryptographically protected
4906   data. Information that may be considered KRI includes the keying material to be recovered
4907   or sufficient information to reconstruct the keying material, other associated key
4908   information, the time when the key was created, the identifier associated with the owner of
4909   the key (i.e., the individual, application, or organization that created the key or owns the data
4910   protected by that key), and any conditions that must be met by a requestor to be able to
4911   recover the keying material.
4912   When an organization determines that key recovery is required for all or part of its keying
4913   material, a secure key-recovery system (KRS) needs to be established in accordance with a
4914   well-defined key recovery policy (see Appendix B.1.5). The KRS shall support the key recovery
4915   policy and consists of the techniques and facilities for saving and recovering the keying
4916   material, the procedures for administering the system, and the personnel associated with the
4917   system.
4918   When key recovery is determined to be necessary, the KRI may be stored either within an
4919   organization (e.g., in backup or archive storage) or at a remote site by a trusted entity. There
4920   are many acceptable methods for enabling key recovery. A KRS 1) could be established using
4921   a safe to store keying material; 2) might use a single computer that provides the initial
4922   protection of the plaintext data, storage for the associated keying material, and recovery of
4923   that keying material; 3) may include a network of computers with a central key-recovery
4924   center; or 4) could be designed using other configurations. Since a KRS provides a means for
4925   recovering cryptographic keys, a risk assessment should be performed to ensure that the KRS
4926   adequately protects the organization’s information and reliably provides the KRI when
4927   required. It is the responsibility of the organization that needs to provide key recovery to
4928   ensure that the key-recovery policy, key-recovery methodology, and KRS adequately protect
4929   the KRI.
4930   A KRS used by the Federal Government shall:
4931       1. Generate or provide sufficient KRI to allow for the recovery or verification of
4932          protected information
4933       2. Ensure the validity of the saved key and other KRI
4934       3. Ensure that the KRI is stored with persistence and availability that is commensurate
4935          with that of the corresponding cryptographically protected data
4936       4. Use cryptographic modules that are compliant with [FIPS 140-3]
4937       5. Use approved algorithms when cryptography is used




                                                        152


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                                Recommendation for Key Management
       December 2025                                                                                                    Part 1 — General

4938        6. Use algorithms and key lengths that provide security strengths commensurate with
4939           the sensitivity of the information associated with the KRI
4940        7. Be designed to enforce the key recovery policy (see Appendix B.1.5)
4941        8. Protect the KRI against unauthorized disclosure or destruction, verify the source of
4942           requests, and ensure that only requested and authorized information is provided to
4943           the requestor
4944        9. Protect the KRI from modification
4945        10. Have the capability to provide an audit trail that shall not contain the keys that are
4946            recovered or any passwords that may be used by the system and that should include
4947            the identification of the event being audited, the time of the event, the identifier
4948            associated with the entity causing the event, and the success or failure of the event
4949        11. Limit access to the KRI, the audit trail, and authentication data to authorized
4950            individuals
4951        12. Prohibit modification of the audit trail

4952   B.1.5. Key-Recovery Policy
4953   For each system, application, and cryptographic technique used, a determination must be
4954   made about whether the keying material needs to be saved for later recovery to allow for
4955   subsequent decryption or verification of the information protected by the keying material.
4956   An organization that determines that key recovery is required for some or all of its keying
4957   material should develop a key- recovery policy that addresses the protection and continued
4958   accessibility of that information 54 (see [FPKI-KRP]). The policy should answer the following
4959   questions (at a minimum):
4960        1. What keying material needs to be saved for a given application? For example, keys
4961           and IVs used for the decryption of stored information may need to be saved. Keys
4962           used for the authentication of stored or transmitted information may also need to be
4963           saved.
4964        2. How and where will the keying material be saved? For example, the keying material
4965           could be stored in a safe by the individual who initiates the protection of the data
4966           (e.g., the encrypted data) or automatically saved when the protected data is
4967           transmitted, received, or stored. The keying material could be saved locally or at some
4968           remote site.
4969        3. Who will be responsible for protecting the KRI? For example, each individual,
4970           organization, or sub-organization could be responsible for their own keying material,
4971           or an external organization could perform this function.
4972        4. Who is authorized to receive the KRI upon request and under what conditions? For
4973           example, the individual who protected the information (i.e., used and stored the KRI)

       54 In the case of a PKI, an organization’s key-recovery policy may be included in its PKI Certificate Policy.




                                                                             153


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

4974            or the organization to which the individual is assigned could recover the keying
4975            material. Legal requirements may need to be considered. An organization could
4976            request the information when the individual who stored the KRI is not available.
4977       5. Under what conditions can the policy be modified and by whom?
4978       6. What audit capabilities and procedures will be included in the KRS? The policy shall
4979          identify the events to be audited. Auditable events might include KRI requests and
4980          their associated responses; who made a request and when; the startup and shutdown
4981          of audit functions; the operations performed to read, modify, or destroy the audit
4982          data; requests to access entity authentication data; and the uses of authentication
4983          mechanisms.
4984       7. How will the KRS deal with aged keying material whose security strength has been
4985          reduced below an acceptable level?
4986       8. Who will be notified when keying material is recovered and under what conditions?
4987          For example, the individual who encrypted data and stored the KRI could be notified
4988          when the organization recovers the decryption key because the person is absent, but
4989          the individual might not be notified when the organization is monitoring the activities
4990          of that individual.
4991       9. What procedures need to be followed when the KRS or some portion of the data
4992          within the KRS is compromised?

4993   B.2. Digital Signature Key Pair
4994   The private key of a digital signature key pair (i.e., the private signature key) is used by the
4995   owner of the key pair to apply digital signatures to information. The corresponding public key
4996   (i.e., the public signature-verification key) is used by relying entities to verify the digital
4997   signature.

4998   B.2.1. Private Signature Key
4999   In general, a private signature key shall not be archived (see Table 12). Key backup is not
5000   usually desirable for the private key of a signing key pair since non-repudiation of the
5001   signature comes into question. However, exceptions may exist. For example, replacing the
5002   private signature key and having its corresponding public signature-verification key
5003   distributed in a timely manner in accordance with Sec. 7.1.5.1 may not be possible under
5004   some circumstances, so recovering the private signature key from backup storage may be
5005   justified. This may be the case, for example, for the private signature key of a CA.
5006   If backup is considered for the private signature key, an assessment should be made about
5007   its importance and the time needed to recover the key as opposed to the time needed to
5008   generate a new key pair and certify and distribute a new public signature-verification key. If
5009   a private signature key is backed up, the private signature key shall be recovered using a




                                                        154


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

5010   highly secure method. Depending on circumstances, the key should be recovered for
5011   immediate use only and shall then be replaced as soon after the recovery process as possible.
5012   Instead of backing up the private signature key, a second private signature key and
5013   corresponding public key could be generated and the public signature-verification key
5014   distributed in accordance with Sec. 7.1.5.1 for use if the primary private signature key
5015   becomes unavailable.

5016   B.2.2. Public Signature-Verification Key
5017   It is appropriate to back up or archive a public signature-verification key for as long as
5018   required to verify the information signed by the corresponding private signature key. In the
5019   case of a public key that has been certified (e.g., by a CA), saving the public-key certificate
5020   would be an appropriate form of storing the public key. Backup or archive storage may be
5021   provided by the infrastructure (e.g., by a certificate repository). The public key should be
5022   stored in backup storage until the end of the private key’s cryptoperiod and should be stored
5023   in archive storage as long as required for the verification of signed data.

5024   B.3. Authentication Keys
5025   Authentication keys are used to provide assurance of the integrity and source of information.

5026   B.3.1. Symmetric Authentication Key
5027   A symmetric authentication key is used to provide assurance of the integrity and source of
5028   information. A symmetric authentication key can be used:
5029       1. By an originator to create a MAC that can be verified to determine the integrity and
5030          possibly the source of the authenticated information. The authenticated information
5031          and its MAC could then be stored for later retrieval or transmitted to another entity.
5032            The symmetric authentication key need not be backed up or archived if the originator
5033            can establish a new authentication key prior to computing the MAC, making the key
5034            available to any entity that would need to verify the information that is authenticated
5035            using this new key. If a new authentication key cannot be established in a timely
5036            manner, then the authentication key should be backed up or archived.
5037       2. By a receiving entity immediately upon receipt of an authenticated message to
5038          determine the integrity of the received information and the source of that
5039          information. The received MAC and the associated authenticated information may or
5040          may not be subsequently stored.
5041            The symmetric authentication key need not be backed up or archived if the
5042            authentication key can be securely provided to the recipient with assurance of the
5043            identity of the sending entity. Alternatively, establishing a new symmetric
5044            authentication key rather than reusing the “lost” key is also acceptable. However, a
5045            new MAC would need to be computed on the information using the new


                                                        155


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

5046            authentication key. If neither alternative is acceptable, the symmetric authentication
5047            key should be backed up.
5048            If the MAC and the successfully authenticated information are subsequently stored,
5049            then the symmetric authentication key should be backed up or archived for as long
5050            as the integrity and source of the information needs to be determined.
5051       3. By an entity that retrieves the authenticated information and the MAC from storage
5052          to determine the integrity of the stored information.
5053            The symmetric authentication key should be backed up or archived for as long as the
5054            integrity and source of the information needs to be determined.
5055   The symmetric authentication key may be stored in backup storage for the cryptoperiod of
5056   the key and in archive storage until no longer required. If the authentication key is recovered
5057   by reconstruction, the “base” key (e.g., the master/key-derivation key for a key-derivation
5058   method or a seed) may be stored in normal operational storage or backup storage for the
5059   cryptoperiod of the base key and in archive storage until no longer required.

5060   B.3.2. Authentication (Asymmetric) Key Pair
5061   A public authentication key is used by a receiving entity to obtain assurance of the identity of
5062   the originating entity (i.e., the owner of the key pair). The corresponding private
5063   authentication key is used by the originating entity to provide this assurance to a receiving
5064   entity by computing a digital signature on information. This key pair may not provide support
5065   for non-repudiation.

5066   B.3.2.1. Private Authentication Key
5067   A private authentication key is used to establish the identity of an entity (i.e., the owner of
5068   the key pair) who is participating in an authenticated communication session. The private
5069   authentication key need not be backed up if a new key pair can be generated and distributed
5070   in a timely manner in accordance with Sec. 7.1.5.1. However, if a new key pair cannot be
5071   generated quickly, the private key should be stored in backup storage during the
5072   cryptoperiod of the private key. The private key shall not be stored in archive storage.

5073   B.3.2.2. Public Authentication Keys
5074   It is appropriate to store a public authentication key in either backup or archive storage for
5075   as long as required to verify the identity of the owner of the key pair that is participating in
5076   an authenticated communication session.
5077   In the case of a public authentication key that has been certified (e.g., by a CA), saving the
5078   public-key certificate would be an appropriate form of storing the public key; backup or
5079   archive storage may be provided by the infrastructure (e.g., by a certificate repository). The
5080   public key may be stored in backup storage until the end of the private authentication key’s
5081   cryptoperiod and may be stored in archive storage as long as required.


                                                        156


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

5082   B.4. Random Number Generation Key
5083   A key used for random bit generation shall not be backed up or archived. If this key is lost or
5084   modified, it shall be replaced with a new key.

5085   B.5. Key-Derivation/Master Key
5086   A symmetric key-derivation/master key is normally used to derive one or more keys or other
5087   keying material. It shall not be used for any other purpose.
5088   The determination as to whether a symmetric key-derivation/master key needs to be backed
5089   up or archived depends on several factors:
5090       1. How easy is it to establish a new symmetric key-derivation/master key? If the key is
5091          distributed manually (e.g., in smart cards or in hard copy by receipted mail), the key
5092          should be backed up or archived. If a new key can be easily and quickly established
5093          using automated key-establishment protocols, then the backup or archiving of the key
5094          may not be necessary or desirable, depending on the application.
5095       2. Are the derived keys recoverable without the use of the symmetric key-
5096          derivation/master key? If the derived keys do not need to be backed up or archived
5097          (e.g., because of their use) or recovery of the derived keys does not depend on
5098          reconstruction from the key-derivation/master key (e.g., the derived keys are stored
5099          in an encrypted form), then the backup or archiving of the key may not be desirable.
5100          If the derived keys need to be backed up or archived and the method of key recovery
5101          requires a reconstruction of the derived key from the key-derivation/master key, then
5102          the key-derivation/master key should be backed up or archived.

5103   B.6. Key Establishment (Automated)

5104   B.6.1. Symmetric Key-Wrapping Key
5105   A symmetric key-wrapping key is used to wrap (i.e., encrypt and integrity protect) keying
5106   material for transmission in one or more messages.
5107   The backup of a symmetric key-wrapping key should be considered if a new symmetric key-
5108   wrapping key cannot be readily and securely established with assurances of the identities of
5109   the sharing entities.
5110   The archive of a symmetric key-wrapping key that is only used to transmit keying material
5111   may not be necessary.

5112   B.6.2. Key-Transport Key Pair
5113   A key-transport key pair is used to transmit keying material from a sending entity to a
5114   receiving entity during communications. The sending entity uses the public key-transport key



                                                        157


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

5115   to encrypt the keying material; the receiving entity uses the private key to decrypt the
5116   received, encrypted keying material.

5117   B.6.2.1. Public Key-Transport Key
5118   The backup or archiving of a public key-transport key may be done but may not be necessary.
5119   If the sending entity loses the other party’s public key-transport key or determines that the
5120   key has been corrupted, the key can be reacquired from the key-pair owner (i.e., the other
5121   party) if the sending entity has assurance of the identity of the party actually providing the
5122   public key or by obtaining a public-key certificate containing the public key (if the public key
5123   was certified).

5124   B.6.2.2 Private Key-Transport Key
5125   A private key-transport key should be backed up or archived unless 1) a new key pair can be
5126   quickly generated and the public key provided to a sending entity (with assurance of the
5127   identity of the key-pair owner providing the new public key) or 2) an alternative key pair has
5128   already been generated and the public key made available (e.g., in a certificate).
5129   If the received, encrypted keying material is stored for later decryption, then the private key-
5130   transport key should be backed up or archived. See Appendix B.8.2 for a discussion of stored
5131   keys.

5132   B.6.3. Symmetric Key-Agreement Key
5133   A symmetric key-agreement key is used during communications to establish keying material
5134   (e.g., symmetric key-wrapping keys, symmetric data-encryption keys, symmetric
5135   authentication keys, or IVs). Each key-agreement key is shared between two or more entities.
5136   If these keys are distributed manually (e.g., in a key-loading device or by receipted mail), then
5137   the symmetric key-agreement key should be backed up.
5138   If an automated means is available for quickly establishing new keys (e.g., a key-transport or
5139   key-encapsulation mechanism can be used to establish a new symmetric key-agreement key),
5140   then a symmetric key-agreement key need not be backed up.
5141   Symmetric key-agreement keys may be archived.

5142   B.6.4. Static Key-Agreement Key Pair
5143   A static key-agreement key pair is used to establish symmetric keying material between two
5144   entities (see [SP 800-56A] and [SP 800-56B]), sometimes in conjunction with ephemeral key
5145   pairs (see Appendix B.6.5 and [SP 800-56A]). In some key-agreement schemes, one of the
5146   entities (designated as the originating entity in [SP 800-56A]) uses only an ephemeral key pair
5147   rather than a static key-agreement key pair.




                                                        158


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)              Recommendation for Key Management
       December 2025                                                                  Part 1 — General

5148   In these schemes, each entity uses the other party’s public keys, their own private keys, and
5149   (in some schemes) their own public keys to generate shared keying material.

5150   B.6.4.1. Public Static Key-Agreement Key
5151   A public static key-agreement key need not be backed up if:
5152       1. The public key has been certified and can be obtained on request;
5153       2. The public key can be obtained from another entity (e.g., the owner of the key pair
5154          associated with the public static key-agreement key) with assurance of the owner’s
5155          identity;
5156       3. The entity intending to use the public key in a key-agreement computation is the
5157          owner of the static key-agreement key pair, and the public key can be recomputed
5158          (e.g., using the private static key-agreement key); or
5159       4. The entity intending to use the public key in a key-agreement computation is the
5160          owner of the key pair, and a new key pair can be generated and securely established
5161          in a timely fashion (i.e., with assurance of the identity of the owner). A newly
5162          generated public static key-agreement key can then be provided to the other entity
5163          participating in the key-agreement process.
5164   Otherwise, the public static key-agreement key should be backed up.
5165   The public key may be archived.

5166   B.6.4.2. Private Static Key-Agreement Key
5167   If a private static key-agreement key cannot be replaced in a timely manner, or if it needs to
5168   be retained to recover previously agreed-upon keying material, then the private key should
5169   be backed up in order to continue operations.
5170   The private key may be archived.

5171   B.6.5. Ephemeral Key-Agreement Key Pair
5172   An ephemeral key-agreement key pair is generated and the public key distributed during a
5173   single key-agreement transaction (e.g., at the beginning of a communication session) and
5174   must not be reused. This key pair is used to establish shared keying material (often in
5175   combination with static key pairs). Not all key-agreement schemes use ephemeral key pairs,
5176   and when used, not all entities use an ephemeral key pair (see [SP 800-56A]).

5177   B.6.5.1. Public Ephemeral Key-Agreement Key
5178   A public ephemeral key-agreement key may be backed up or archived. This may allow for the
5179   reconstruction of the established keying material at a later time as long as the corresponding
5180   private ephemeral key is not required in the reconstruction computation.


                                                        159


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                                         Recommendation for Key Management
       December 2025                                                                                             Part 1 — General

5181   B.6.5.2. Private Ephemeral Key-Agreement Key
5182   A private ephemeral key-agreement key shall not be backed up or archived. 55 If the private
5183   ephemeral key is lost or corrupted, a new key pair may be generated, and the new public
5184   ephemeral key may be provided to the other participating entity in the key-agreement
5185   process.

5186   B.6.6. Static Encapsulation/Decapsulation Key Pair
5187   A public static encapsulation key is used by a sending entity to generate and encapsulate a
5188   symmetric key to be provided to the owner of the encapsulation/decapsulation key pair. The
5189   owner of the key pair uses the corresponding private static decapsulation key to decapsulate
5190   the received, encapsulated key.

5191   B.6.6.1. Public Static Encapsulation Key
5192   Backing up or archiving the public static encapsulation key may not be necessary if the key
5193   can be reacquired from the key-pair owner or by obtaining the public-key certificate that
5194   contains the public static encapsulation key (assuming that the public key was certified).
5195   Otherwise, a new public static key encapsulation key may be requested from the intended
5196   receiving entity.

5197   B.6.6.2. Private Static Decapsulation Key
5198   A private static decapsulation key should be backed up or archived until no longer needed.

5199   B.6.7. Ephemeral Encapsulation/Decapsulation Key Pair
5200   An ephemeral encapsulation/decapsulation key pair is generated and used during a single
5201   key-establishment transaction and shall not be reused (see [SP 800-227]). A public ephemeral
5202   encapsulation key is used by a sending entity to generate and encapsulate a symmetric key
5203   to be provided to the owner of the encapsulation/decapsulation key pair. The owner of the
5204   key pair uses the corresponding private ephemeral decapsulation key to decapsulate the
5205   received, encapsulated key.

5206   B.6.7.1. Public Ephemeral Encapsulation Key
5207   A public ephemeral encapsulation key need not be backed up or archived since it is only used
5208   for a single key-establishment transaction and then destroyed. If unavailable, the owner of
5209   the ephemeral encapsulation/decapsulation key pair can be asked to resend the public key



       55 [SP 800-56A] states that the private ephemeral keys shall be destroyed immediately after use. This implies that the private ephemeral

       keys shall not be backed up or archived.



                                                                        160


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

5210   or to generate another key pair and provide the new public ephemeral encapsulation key
5211   (with assurance of the identity from the owner in both cases).

5212   B.6.7.2. Private Ephemeral Decapsulation Key
5213   A private ephemeral decapsulation key need not be backed up by the owner if it is intended
5214   to be used immediately upon receipt of a key that was encapsulated using the corresponding
5215   public ephemeral encapsulation key. If the private key is unavailable at that time, then a new
5216   ephemeral key pair can be generated and the public key provided to the other entity.
5217   If the received key is not decapsulated upon receipt of the encapsulated key or the
5218   decapsulation key may be used to decapsulate the received key at a later time, then the
5219   private ephemeral decapsulation key should be backed up or archived (see Appendix B.8 for
5220   a discussion of keys in storage).

5221   B.7. Symmetric Data Encryption/Decryption Keys
5222   A symmetric data-encryption key is used to protect the confidentiality of transmitted or
5223   stored data. The same key is used to encrypt the plaintext data to be protected and later to
5224   decrypt the encrypted data (i.e., the ciphertext), thus obtaining the original plaintext.
5225   The key needs to be available for as long as any data that is encrypted using that key may
5226   need to be decrypted.

5227   B.7.1. Encryption/Decryption of Data in Transit
5228   A symmetric data-encryption key that is used only for transmission is used by a sending entity
5229   to encrypt data and by the receiving entity to decrypt the ciphertext data.
5230       •    Case 1: The sending entity no longer has an encryption key that is shared with the
5231            intended receiver. If a new encryption/decryption key can be readily generated and
5232            distributed, then these keys need not be backed up or archived. Otherwise, the keys
5233            should be backed up or archived until no longer needed.
5234       •    Case 2: The receiving entity always decrypts data immediately upon receipt. The
5235            encryption/decryption key should be backed up or archived until no longer needed
5236            (see Appendices B.7.2 and B.8) unless a new encryption/decryption key can be readily
5237            generated and distributed.
5238       •    Case 3: The receiving entity stores the received ciphertext for later decryption. The
5239            encryption/decryption key should be backed up or archived until no longer needed
5240            (see Appendices B.7.2 and B.8).

5241   B.7.2.     Encryption/Decryption of Data at Rest
5242   The symmetric data-encryption key should be stored in backup storage during the
5243   cryptoperiod of the key and should be stored in archive storage if required beyond the


                                                        161


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                           Recommendation for Key Management
       December 2025                                                                               Part 1 — General

5244   originator-usage period of the key. In many cases, the key is protected and stored with the
5245   encrypted data (see Appendix B.8).

5246   B.8. Keying Material Storage
5247   Keys and other related information (i.e., KRI) may be stored in operational storage within the
5248   computer (e.g., RAM), within hardware designed to protect the keys, or on external storage
5249   media (e.g., backed up or archived for storage at a remote location) (see Sec. 5.2.2.7). When
5250   protection is not inherently provided by the storage medium, the confidentiality and integrity
5251   of the stored key needs to be assured using a backup or archive confidentiality key to encrypt,
5252   wrap, or encapsulate the stored key and possibly an integrity protection key when the
5253   integrity protection is not provided by the encrypting, wrapping, or encapsulation process.
5254   The key-storage medium may be organized as a tree structure with multiple layers of keys
5255   (see Fig. 6).




5256
5257                                      Fig. 6. Example of a tree of keys in storage

5258   Presumably, the lowest layer of keys is used to encrypt data other than keys. The security of
5259   each layer depends (in part) on the security of the layers above it.
5260   Using the example depicted in Fig. 6, the highest layer key could be an asymmetric key-
5261   wrapping key pair (e.g., an RSA key pair) used for confidentiality protection for the next layer
5262   of keys. A different key pair might be used for each organization. In Fig. 6, the public wrapping
5263   key for Organization 1 might be eKey1, and the corresponding private unwrapping key might


                                                              162


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

5264   be dKey1. Organization 1 has several applications and uses a symmetric key-wrapping
5265   algorithm (e.g., AES-CCM) with different key-wrapping keys (shown as KeyA, KeyB, and KeyC)
5266   to protect data keys for each application (i.e., applications A, B, and C, respectively).
5267   Application B has generated multiple data keys to protect its data (DEK1, DEK2, …) using AES-
5268   CCM and its wrapping key KeyB. In this example, data key DEK1 has been used to encrypt
5269   plaintext data using AES-CCM:
5270                      EncryptedData = ENCRYPTAES-CCM(DEK1, PlaintextData).                       (1)
5271   DEK1 is wrapped using AES-CCM and KeyB:
5272                               Wrapped DEK1 = WRAPAES-CCM(KeyB, DEK1),                           (2)
5273   and KeyB is wrapped using RSA and eKey1:
5274                               Wrapped KeyB = WRAPRSA(eKey1, KeyB).                              (3)
5275   Wrapped KeyB, Wrapped DEK1, and the EncryptedData are stored. The highest-layer key in
5276   the tree (dKey1) shall be securely stored offline when not protected by the storage system
5277   (e.g., in a safe with dual access control).
5278   In order to access the data protected by DEK1 (i.e., the plaintext data), the private unwrapping
5279   key (dKey1) is used to unwrap Organization 1’s symmetric-unwrapping key KeyB:
5280                               KeyB = UNWRAPRSA(dkey1, Wrapped KeyB),                            (4)
5281   the symmetric unwrapping key (KeyB) is then used to unwrap DEK1:
5282                               DEK1 = UNWRAPAES-CCM(KeyB, Encrypted DEK1),                       (5)
5283   and DEK1 is used to decrypt the data it protects.
5284                          PlaintextData = DECRYPTAES-CCM(DEK1, EncryptedData).                   (6)

5285   B.8.1. Symmetric Key Wrapping/Unwrapping Key
5286   A symmetric key-wrapping key is used to wrap (i.e., encrypt and integrity protect) keying
5287   material for storage and may be used to protect multiple sets of keying material (see equation
5288   2 in Appendix B.8, for example). The symmetric key-wrapping key should be archived or
5289   stored offline (e.g., in a safe with dual access control) until no longer needed to unwrap keying
5290   material previously wrapped using the key.

5291   B.8.2. Asymmetric Key Encryption/Decryption Key Pair
5292   An asymmetric public key-encryption key may be used to encrypt keying material for storage
5293   (see equation 3 in Appendix B.8, for example). The corresponding private decryption key is
5294   used to decrypt the keying material, when needed (see equation 6 in Appendix B.8, for
5295   example).




                                                        163


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
       December 2025                                                                     Part 1 — General

5296   B.8.2.1. Public Key-Wrapping Key
5297   Backing up or archiving a public key-wrapping key is not necessary, assuming that another
5298   key encryption/decryption key pair can be readily generated.

5299   B.8.2.2. Private Key-Unwrapping Key
5300   A private key-unwrapping key should be backed up or archived for as long as the keying
5301   material encrypted by the corresponding public key-encryption key needs to be accessed.

5302   B.8.3. Encapsulation/Decapsulation Key Pair
5303   An encapsulation key could be used to encapsulate a symmetric key for protecting keys or
5304   data in storage. The corresponding decapsulation key would be used to decapsulate the
5305   encapsulated key.

5306   B.8.3.1. Public Encapsulation Keys
5307   Backing or archiving the public encapsulation key is not necessary, assuming that another
5308   encapsulation/decapsulation key pair can be readily generated.

5309   B.8.3.2. Private Decapsulation Keys
5310   A private decapsulation key should be backed up, archived, or stored offline for as long as
5311   the encapsulated keying material may need to be accessed.

5312   B.9. Authorization

5313   B.9.1. Symmetric Authorization Key
5314   A symmetric authorization key is used to provide privileges to an entity (e.g., access to certain
5315   data or authorization to perform certain functions). The loss of this key will deny the privileges
5316   (e.g., prohibit access and disallow the performance of these functions). If the authorization
5317   key is lost or corrupted and can be replaced in a timely fashion, then the authorization key
5318   need not be backed up. A symmetric authorization key shall not be archived.

5319   B.9.2. Authorization Key Pair
5320   An authorization key pair is used to determine the privileges that an entity may assume. The
5321   private key is used to establish the “right” to the privilege. The corresponding public key is
5322   used to determine that the entity has the right to the privilege.




                                                        164


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
       December 2025                                                                     Part 1 — General

5323   B.9.2.1. Private Authorization Key
5324   The loss of a private authorization key will deny privileges (e.g., prohibit access and disallow
5325   the performance of certain functions requiring authorization). If the private key is lost or
5326   corrupted and can be replaced in a timely fashion, then the private key need not be backed
5327   up. Otherwise, the private key should be backed up. The private key shall not be archived.

5328   B.9.2.2. Public Authorization Key
5329   If an authorization key pair can be replaced in a timely fashion (i.e., by a regeneration of the
5330   key pair and secure distribution of the private key to the entity seeking authorization), then
5331   the public authorization key need not be backed up. Otherwise, a public authorization key
5332   should be backed up. There is no need to archive the public authorization key, since
5333   authorization is only granted using the corresponding private authorization key, which is not
5334   archived (see Appendix B.9.2.1).

5335   B.10. Other Related Information
5336   Like keys, other related information may need to be backed up or archived, depending on its
5337   use.

5338   B.10.1. Algorithm Parameters
5339   Algorithm parameters are used in conjunction with some public-key algorithms to generate
5340   key pairs or perform basic algorithm functions. Algorithm parameters should be backed up
5341   or archived if not obtainable from another trusted source (e.g., a PKI certificate) or if any new
5342   algorithm parameters cannot be securely distributed in a timely fashion.

5343   B.10.2. Initialization Vector (IV)
5344   An IV is used in several modes of operation during the encryption or authentication of data
5345   using block cipher algorithms. An IV is often stored with the data that it protects. If not stored
5346   with the data, an IV should be backed up or archived as long as the data protected using the
5347   IV needs to be processed (e.g., decrypted or authenticated).

5348   B.10.3. Shared Secret
5349   A shared secret is generated by each entity that participates in a key-agreement process. The
5350   shared secret is then used to derive the shared keying material to be used in subsequent
5351   cryptographic operations. A shared secret may be generated during interactive
5352   communications (e.g., where both entities are online) or during non-interactive
5353   communications (e.g., in store-and-forward applications).
5354   A shared secret shall not be backed up or archived.



                                                        165


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                 Recommendation for Key Management
       December 2025                                                                     Part 1 — General

5355   B.10.4. Seed
5356   A seed is used for the generation of random bits or keying material. When used for the
5357   generation of random bits, the seed shall not be backed up or archived but may be replaced.
5358   When used for the regeneration (i.e., reconstruction) of keying material, the seed may be
5359   backed up or archived.

5360   B.10.5. Other Public and Secret Information
5361   Public and secret information is often used during key establishment. The information may
5362   need to be available to identify the key that is needed to process cryptographically protected
5363   data (e.g., to decrypt or authenticate). In this case, the information should be backed up or
5364   archived until no longer needed to process the protected data.

5365   B.10.6. Intermediate Results
5366   Except for the generation and use of seeds (see Appendix B.10.4), the intermediate results of
5367   a cryptographic operation shall not be backed up or archived.

5368   B.10.7. Key-Control Information/Metadata
5369   Key-control information may be used to determine the key and other information to be used
5370   to process cryptographically protected data (e.g., decrypt or authenticate), identify the
5371   purpose of the key, or identify the entities that share the key (see Sec. 5.2.3). This information
5372   may be contained in the key’s metadata.
5373   Key-control information should be backed up or archived for as long as the associated key
5374   needs to be available.

5375   B.10.8. Random Numbers
5376   A random number is generated by a random number generator. The backup or archiving of a
5377   random number depends on how it is used.

5378   B.10.9. Password
5379   A password is used to acquire access to privileges by an entity, derive a key, or detect the re-
5380   use of a password.
5381   If the password is only used to acquire access to privileges and can be replaced in a timely
5382   fashion, then the password need not be backed up. In this case, a password shall not be
5383   archived.
5384   If the password is used to derive a cryptographic key or prevent the reuse of a password, the
5385   password should be backed up and archived.



                                                        166


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)          Recommendation for Key Management
       December 2025                                                              Part 1 — General

5386   B.10.10. Audit Information
5387   Audit information containing key-management events shall be backed up and archived.
5388




                                                        167


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                      Recommendation for Key Management
       December 2025                                                                          Part 1 — General

5389   Appendix C. Security Strength Categories for Post-Quantum Algorithms
5390   There are potentially significant uncertainties in estimating the security strengths of post-
5391   quantum cryptosystems. New quantum algorithms may be discovered, leading to new
5392   cryptanalytic attacks, and there is a limited ability to predict the performance characteristics
5393   of future quantum computers, such as their cost, speed, and memory size.
5394   In order to address these uncertainties, NIST developed a collection of broad security
5395   strength categories 56 in order to evaluate post-quantum algorithm candidates [NIST PQC
5396   eval]. Each category is defined by a comparatively easy-to-analyze reference primitive whose
5397   security serves as a floor for a wide variety of metrics that are potentially relevant to practical
5398   security. A given cryptosystem may be instantiated using different parameter sets in order to
5399   fit into different categories. The goals of this classification are to:
5400        1. Facilitate meaningful performance comparisons between the submitted algorithms
5401           by striving to ensure that the parameter sets being compared provide comparable
5402           security
5403        2. Enable an organization to make prudent future decisions regarding when to transition
5404           to longer keys
5405        3. Permit consistent and sensible choices regarding what symmetric primitives to use in
5406           padding mechanisms or other components of schemes that require symmetric
5407           cryptography
5408        4. Better understand the security and performance trade-offs involved in a given
5409           design approach
5410   In accordance with the second and third goals above, the categorization approach is based
5411   on the range of security strengths offered by existing NIST standards in symmetric
5412   cryptography, which have been identified as offering significant resistance to quantum
5413   cryptanalysis. The approach also considers a variety of possible metrics that reflect different
5414   predictions about the future development of quantum and classical computing technology.
5415   The categorization approach considers a variety of possible metrics, reflecting different
5416   predictions about the future development of quantum and classical computing technology.
5417   One of the metrics for measuring the complexity of quantum attacks during the NIST PQC
5418   standardization process is designated as MAXDEPTH, which was motivated by the difficulty
5419   of running extremely long serial computations. Plausible values for MAXDEPTH range from
5420   240 logical gates (i.e., the approximate number of gates that quantum computing
5421   architectures were expected to serially perform in a year) through 264 logical gates (i.e., the
5422   approximate number of gates that classical computing architectures could serially perform in
5423   a decade) to no more than 296 logical gates (i.e., the approximate number of gates that atomic
5424   scale qubits with speed of light propagation times could perform in a millennium) [Jones].
5425   The complexity of quantum attacks can be measured in terms of circuit size (see [Grassl]).
5426   These numbers can be compared to the resources required to break AES and SHA3. NIST gave

       56 The five defined categories are specified in Sec. 4.6.1.




                                                                     168


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)               Recommendation for Key Management
       December 2025                                                                   Part 1 — General

5427   the following estimates for classical and quantum gate counts for optimal key recovery and
5428   collision attacks on AES and SHA3, respectively, where circuit depth is limited to MAXDEPTH:
5429       •    AES 128           2170/MAXDEPTH quantum gates or 2143 classical gates
5430       •    SHA3-256          2146 classical gates
5431       •    AES 192           2233/MAXDEPTH quantum gates or 2207 classical gates
5432       •    SHA3-384          2210 classical gates
5433       •    AES 256           2298/MAXDEPTH quantum gates or 2272 classical gates
5434       •    SHA3-512          2274 classical gates
5435   NIST believes that these estimates are accurate for the majority of values of MAXDEPTH that
5436   are relevant to the security analysis of the post-quantum algorithms, but the estimates may
5437   understate the security of SHA for very small values of MAXDEPTH and may understate the
5438   quantum security of AES for very large values of MAXDEPTH.




                                                         169


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)         Recommendation for Key Management
       December 2025                                                             Part 1 — General

5439   Appendix D. List of Abbreviations and Acronyms
5440   2TDEA
5441   Two-Key Triple Data Encryption Algorithm

5442   3TDEA
5443   Three-key Triple Data Encryption Algorithm

5444   AEAD
5445   Authenticated Encryption with Associated Data

5446   AES
5447   Advanced Encryption Standard

5448   ANS
5449   American National Standard

5450   ANSI
5451   American National Standards Institute

5452   CA
5453   Certification Authority

5454   CAVP
5455   Cryptographic Algorithm Validation Program

5456   CKL
5457   Compromised Key List

5458   CRC
5459   Cyclic Redundancy Check

5460   CRL
5461   Certificate Revocation List

5462   CMAC
5463   Cipher-Based Message Authentication Code

5464   CMVP
5465   Cryptographic Module Validation Program

5466   DES
5467   Data Encryption Standard

5468   DH
5469   Diffie-Hellman

5470   DRBG
5471   Deterministic Random Bit Generator

5472   DSA
5473   Digital Signature Algorithm

5474   ECC
5475   Elliptic Curve Cryptography




                                                        170


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                     Recommendation for Key Management
       December 2025                                                                         Part 1 — General

5476   ECDH
5477   Elliptic Curve Diffie-Hellman

5478   ECDSA
5479   Elliptic Curve Digital Signature Algorithm

5480   EdDSA
5481   Edwards-Curve Digital Signature Algorithm

5482   FFC
5483   Finite Field Cryptography

5484   FIPS
5485   Federal Information Processing Standards

5486   HMAC
5487   Keyed-Hash Message Authentication Code

5488   HSM
5489   Hardware Security Module

5490   IC
5491   Integrated Circuit

5492   IFC
5493   Integer Factorization Cryptography

5494   IP
5495   Internet Protocol

5496   ISO/ITU-T
5497   International Organization for Standardization/International Telecommunication Union − Telecommunication

5498   ITL
5499   Information Technology Laboratory

5500   IV
5501   Initialization Vector

5502   KMAC
5503   Keccak-Based Message Authentication Code

5504   KRI
5505   Key-Recovery Information

5506   KRS
5507   Key-Recovery System

5508   MAC
5509   Message Authentication Code

5510   ML-DSA
5511   Module-Lattice-Based Digital Signature Standard

5512   ML-KEM
5513   Module-Lattice-Based Key-Encapsulation Mechanism




                                                          171


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)           Recommendation for Key Management
       December 2025                                                               Part 1 — General

5514   MQV
5515   Menezes-Qu-Vanstone

5516   NIST
5517   National Institute of Standards and Technology

5518   PKI
5519   Public-Key Infrastructure

5520   POP
5521   Proof of Possession

5522   PQC
5523   Post-Quantum Cryptography

5524   RA
5525   Registration Authority

5526   RAM
5527   Random Access Memory

5528   RBG
5529   Random Bit Generator

5530   RNG
5531   Random Number Generator

5532   RSA
5533   Rivest–Shamir–Adelma

5534   S/MIME
5535   Secure Multipurpose Internet Mail Extensions

5536   SHA-2
5537   Secure Hash Algorithm 2

5538   SHA-3
5539   Secure Hash Algorithm 3

5540   SHAKE
5541   Secure Hash Algorithm KECCAK

5542   SLH-DSA
5543   Stateless Hash-Based Digital Signature Algorithm

5544   SSH
5545   Secure Shell Protocol

5546   TDEA
5547   Triple Data Encryption Algorithm

5548   TLS
5549   Transport Layer Security

5550   TPM
5551   Trusted Platform Module




                                                          172


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)         Recommendation for Key Management
       December 2025                                                             Part 1 — General

5552   XOF
5553   eXtendable-Output Function

5554   USB
5555   Universal Serial Bus




                                                        173


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                            Recommendation for Key Management
       December 2025                                                                                Part 1 — General

5556     Appendix E. Glossary
5557   access control
5558   Restricts resource access to only authorized entities.

5559   accountability
5560   1. Assigning key management responsibilities to individuals and holding them accountable for these activities.
5561   2. A property that ensures that the actions of an entity may be traced uniquely to that entity.

5562   active state
5563   The key state in which the key may be used to cryptographically protect information (e.g., encrypt plaintext, or
5564   generate a digital signature), cryptographically process previously protected information (e.g., decrypt
5565   ciphertext or verify a digital signature), or both.

5566   algorithm originator-usage period
5567   The period of time during which a specific symmetric-key algorithm may be used by originators to apply
5568   protection to data (e.g., encrypt or generate a MAC).

5569   algorithm parameters
5570   Parameters used in conjunction with some public-key algorithms to generate key pairs or to perform
5571   cryptographic operations (e.g., create digital signatures or establish keying material).

5572   algorithm security lifetime
5573   The estimated time period during which data protected by a specific cryptographic algorithm is estimated to
5574   remain secure, given that the key has not been compromised.

5575   approved
5576   FIPS-approved and/or NIST-recommended. An algorithm or technique that is either 1) specified in a FIPS or NIST
5577   recommendation or 2) specified elsewhere and adopted by reference in a FIPS or NIST recommendation.

5578   archive
5579   1. To place information into long-term storage.
5580   2. A location or media used for long-term storage.

5581   association
5582   A relationship for a particular purpose, such as a key associated with the application or process for which it will
5583   be used.

5584   assurance of (private-key) possession
5585   Confidence that an entity possesses a private key and its associated key information and that the private key
5586   corresponds to a given public key.

5587   assurance of validity
5588   Confidence that a public key or domain parameter is correct.

5589   asymmetric-key algorithm
5590   See public-key cryptographic algorithm.

5591   authentication
5592   A process that provides assurance of the source and integrity of information in communication sessions,
5593   messages, documents, or stored data or that provides assurance of the identity of an entity interacting with a
5594   system. See source authentication, identity authentication, and integrity authentication.




                                                                174


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                              Recommendation for Key Management
       December 2025                                                                                  Part 1 — General

5595   authentication code
5596   A keyed cryptographic checksum based on an approved security function. Also known as a message
5597   authentication code.

5598   authorization
5599   Access privileges that are granted to an entity that convey an “official” sanction to perform a security function
5600   or activity.

5601   availability
5602   Timely, reliable access to information by authorized entities.

5603   backup
5604   A copy of key information to facilitate recovery during the cryptoperiod of the key, if necessary.

5605   block cipher (algorithm)
5606   A symmetric-key cryptographic algorithm that transforms one block of information at a time using a
5607   cryptographic key. For a block cipher algorithm, the length of the input block is the same as the length of the
5608   output block.

5609   certificate
5610   See public-key certificate.

5611   certificate-inventory management
5612   See key-inventory management.

5613   certification authority
5614   A trusted party in a public-key infrastructure (PKI) that issues public-key certificates to certificate subjects.

5615   certificate subject
5616   The entity authorized to use the private key associated with the public key in a public-key certificate.

5617   ciphertext
5618   Data in its encrypted form.

5619   collision
5620   Two or more distinct inputs produce the same output. Also see hash function.

5621   compromise
5622   The unauthorized disclosure, modification, substitution, or use of sensitive key information (e.g., secret key,
5623   private key, or secret metadata).

5624   compromised state
5625   A key state to which a key is transitioned when there is suspicion or confirmation of the key’s compromise.

5626   confidentiality
5627   The property that sensitive information is not disclosed to unauthorized entities (e.g., the secrecy of the key
5628   information is maintained).

5629   contingency plan
5630   A plan that is maintained for disaster response, backup operations, and/or post-disaster recovery to ensure the
5631   availability of critical resources and to facilitate the continuity of operations in an emergency situation.

5632   contingency planning
5633   The development of a contingency plan.




                                                                175


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                          Recommendation for Key Management
       December 2025                                                                              Part 1 — General

5634   cryptanalysis
5635   1. Operations performed to defeat cryptographic protection without initial knowledge of the key employed in
5636      providing the protection.
5637   2. The study of mathematical techniques for attempting to defeat cryptographic techniques and information-
5638      system security. This includes the process of looking for errors or weaknesses in the implementation of an
5639      algorithm or in the design of the algorithm itself.

5640   cryptographic algorithm
5641   A well-defined computational procedure that takes variable inputs, often including a cryptographic key, and
5642   produces an output.

5643   cryptographic boundary
5644   An explicitly defined continuous perimeter that establishes the physical bounds of a cryptographic module and
5645   contains all hardware, software, and/or firmware components of a cryptographic module.

5646   cryptographic element
5647   The cryptographic algorithm, scheme, parameter choice, and/or key of a particular type, length, or strength
5648   used by a cryptographic service.

5649   cryptographic hash function
5650   See hash function.

5651   cryptographic key (key)
5652   A parameter used in conjunction with a cryptographic algorithm that determines its operation in such a way
5653   that an entity with knowledge of the key can reproduce, reverse, or verify the operation while an entity without
5654   knowledge of the key cannot.

5655   cryptographic module
5656   The set of hardware, software, and/or firmware that implements approved security functions and is contained
5657   within a cryptographic boundary. Also see security function.

5658   cryptoperiod
5659   The time span during which a specific key is authorized for use or in which the keys for a given system or
5660   application may remain in effect.

5661   data-encryption key
5662   A key used to encrypt and decrypt data other than keys.

5663   data integrity
5664   A property whereby data has not been altered in an unauthorized manner since it was created, transmitted, or
5665   stored.

5666   decapsulation
5667   A key-establishment procedure in which a symmetric key is produced from ciphertext using a private
5668   decapsulation key and a key-encapsulation mechanism (KEM). See encapsulation and key-encapsulation
5669   mechanism.

5670   decryption
5671   The process of changing ciphertext into plaintext using a cryptographic algorithm and key.

5672   destroy (a key or other data)
5673   An action applied to a key or other data after which no information about its value can be recovered. Other
5674   terms often used are delete, destroy, zeroize, and sanitize.




                                                             176


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                           Recommendation for Key Management
       December 2025                                                                               Part 1 — General

5675   destroyed state
5676   A key state to which a key transitions when it is destroyed. Although the key no longer exists, its previous
5677   existence may be recorded (e.g., in metadata or audit logs).

5678   destruction (of a key or other data)
5679   See destroy.

5680   deterministic random bit generator (DRBG)
5681   A random bit generator that includes a DRBG algorithm and (at least initially) has access to a source of
5682   randomness. The DRBG produces a sequence of bits from a secret initial value called a seed. A cryptographic
5683   DRBG has the additional property that the output is unpredictable given that the seed is not known. A DRBG is
5684   sometimes also called a pseudorandom number generator (PRNG) or a deterministic random number generator.

5685   digital signature
5686   The result of a cryptographic transformation of data that, when properly implemented with a supporting
5687   infrastructure and policy, provides source or identity authentication, data integrity authentication, and/or
5688   support for signer non-repudiation.

5689   encapsulation
5690   A key-establishment procedure in which a random symmetric key and corresponding ciphertext value is
5691   generated using a public encapsulation key and a key-encapsulation mechanism (KEM). See key-encapsulation
5692   mechanism and decapsulation.

5693   encrypted key
5694   A cryptographic key that has been encrypted using an approved cryptographic algorithm to disguise the value
5695   of the underlying plaintext key.

5696   encryption
5697   The process of changing plaintext into ciphertext using a cryptographic algorithm and key.

5698   entity
5699   An individual (person), organization, device, or process.

5700   entity registration
5701   A function in the life cycle of a cryptographic key. A process whereby an entity becomes a member of a security
5702   domain.

5703   ephemeral key
5704   A cryptographic key that is generated for each execution of a cryptographic process (e.g., key establishment)
5705   and that meets other requirements of the key type (e.g., unique to each message or session).

5706   eXtendable-Output Function (XOF)
5707   A function on bit strings in which the length of the output can be extended to any length. Approved XOFs (e.g.,
5708   those specified in [FIPS 202] and [SP 800-232]) are designed to satisfy the following properties as long as the
5709   specified output length is sufficiently long to prevent trivial attacks:
5710       1. (One-way) It is computationally infeasible to find any input that maps to any new pre-specified output.
5711       2. (Collision-resistant) It is computationally infeasible to find any two distinct inputs that map to the same
5712            output.

5713   hash-based message authentication code (HMAC)
5714   A message authentication code that uses an approved hash function and a key (i.e., see [SP 800-224]).

5715   hash function
5716   A function that maps a bit string of arbitrary (although bounded) length to a fixed-length bit string. Approved
5717   hash functions satisfy the following properties:


                                                             177


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                             Recommendation for Key Management
       December 2025                                                                                 Part 1 — General

5718       1. (One-way) It is computationally infeasible to find any input that maps to any pre-specified output.
5719       2. (Collision-resistant) It is computationally infeasible to find any two distinct inputs that map to the same
5720          output.

5721   hashing method
5722   An algorithm that takes an input bit string of arbitrary length and produces an output with a given length. A
5723   hashing method does not require a cryptographic key. Two methods for hashing have been approved:
5724   (cryptographic) hash functions and eXtendable-Output Functions.

5725   hash value
5726   The result of applying a hashing method to information.

5727   identifier
5728   A bit string that is associated with a person, device, or organization. It may be an identifying name or something
5729   more abstract (e.g., a string consisting of an IP address and timestamp), depending on the application.

5730   identity
5731   The distinguishing characteristic or personality of an entity.

5732   identity authentication
5733   The process of providing assurance about the identity of an entity interacting with a system (e.g., to access a
5734   resource). Sometimes called entity authentication.

5735   initialization vector (IV)
5736   A vector used in defining the starting point of a cryptographic process.

5737   integrity
5738   See data integrity.

5739   integrity authentication
5740   The process of obtaining assurance that data has not been modified since an authentication code or digital
5741   signature was created for that data.

5742   integrity protection
5743   The protection obtained for transmitted or stored data using an authentication code (e.g., MAC) or digital
5744   signature computed on that data. See integrity authentication.

5745   key
5746   See cryptographic key.

5747   key agreement
5748   A key-establishment procedure in which keying material is generated from information contributed by two or
5749   more participants so that no party can predetermine the value of the keying material independently of any
5750   other party’s contribution.

5751   key confirmation
5752   A procedure used to provide assurance to one party that another party actually possesses the same keying
5753   material and/or shared secret.

5754   key de-registration
5755   A function in the life cycle of a cryptographic key that involves marking the key or the information associated
5756   with it (e.g., metadata) to indicate that the key is no longer in use.

5757   key derivation
5758   The process by which keying material is derived from either a pre-shared key or a shared secret (from a key-
5759   agreement scheme) along with other information.


                                                               178


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                            Recommendation for Key Management
       December 2025                                                                                Part 1 — General

5760   key-derivation function
5761   A function that generates a binary string called keying material using the input of a cryptographic key or shared
5762   secret and possibly other data.

5763   key-derivation key
5764   A key used with a key-derivation method to derive additional keys. Sometimes called a “master key.”

5765   key-derivation method
5766   A key-derivation function or other approved procedure for deriving keying material.

5767   key destruction
5768   To remove all traces of a cryptographic key so that it cannot be recovered by either physical or electronic means.

5769   key distribution
5770   The transport of a key and other keying material from an entity that either owns, generates, or otherwise
5771   acquires the key to another entity that is intended to use the key.

5772   key-encapsulation mechanism (KEM)
5773   A set of three cryptographic algorithms — KeyGen, Encaps, and Decaps — that can be used by two parties to
5774   establish a shared secret key over a public channel. See encapsulation and decapsulation.

5775   key-encryption key
5776   A cryptographic key that is used for the encryption or decryption of other keys to provide confidentiality
5777   protection for those keys. Also see key-wrapping key.

5778   key establishment
5779   A function in the life cycle of a cryptographic key. The process by which cryptographic keys are securely
5780   established among entities using manual transport methods (e.g., key loaders), automated methods (e.g., key-
5781   transport, key-agreement, or key-encapsulation schemes), or a combination of automated and manual
5782   methods.

5783   key information
5784   Information about a key that includes the keying material and associated metadata related to that key. See
5785   keying material and metadata.

5786   key inventory
5787   Information about each key that does not include the key itself (e.g., key owner, key type, algorithm, application,
5788   or expiration date).

5789   key-inventory (or certificate-inventory) management
5790   Establishing and maintaining records of the keys and/or certificates in use, assigning and tracking their owners
5791   or sponsors, monitoring key and certificate status, and reporting the status to the appropriate official for
5792   remedial action when required.

5793   key length
5794   The length of a key in bits. Used interchangeably with “key size.”

5795   key management
5796   Activities that involve handling cryptographic keys and other related key information during the entire life cycle
5797   of the keys, including their generation, storage, establishment, entry, output, use, and destruction.

5798   key-management policy
5799   A high-level document that identifies a high-level structure, responsibilities, governing standards and
5800   guidelines, organizational dependencies and other relationships, and security policies.




                                                              179


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                          Recommendation for Key Management
       December 2025                                                                              Part 1 — General

5801   key-management system
5802   A system for the management of cryptographic keys and their metadata, including their generation, distribution,
5803   storage, backup, archive, recovery, use, revocation, and destruction. An automated key-management system
5804   may be used to oversee, automate, and secure the key-management process.

5805   key-management practices statement
5806   A document or set of documents that provides detailed descriptions of the organizational structure, responsible
5807   roles, and rules for the functions identified in the key-management policy.

5808   key owner
5809   An entity authorized to use a symmetric key or the private key of a public-key (asymmetric) key pair and whose
5810   identifier is associated with the key or key pair.

5811   key pair
5812   A public key and its corresponding private key that are used with an asymmetric-key (public-key) algorithm.

5813   key recovery
5814   A possible function in the life cycle of a cryptographic key. Mechanisms and processes that allow authorized
5815   entities to retrieve or reconstruct a key.

5816   key registration
5817   A function in the life cycle of a cryptographic key. The process used by a registration authority to create an
5818   official record of keying material.

5819   key revocation
5820   A possible function in the life cycle of a cryptographic key. A process whereby a notice is made available to
5821   affected entities that the key should be removed from operational use prior to the end of the established
5822   cryptoperiod of that key.

5823   key share
5824   One of n parameters (where n ≥ 2) such that among the n key shares, any k key shares (where k ≥ n) can be used
5825   to construct a key value. Having any k−1 or fewer key shares provides no knowledge of the (constructed) key
5826   value. Sometimes called a “cryptographic key component” or “key split.”

5827   key size
5828   The length of a key in bits. Used interchangeably with key length.

5829   key states
5830   The states through which a key transitions between its generation and its destruction. See pre-activation state,
5831   active state, suspended state, compromised state, and destroyed state.

5832   key transport
5833   A key-establishment procedure whereby one party (i.e., the sender) selects and encrypts (or wraps) the key and
5834   then distributes it to another party (i.e., the intended receiver).
5835   When used in conjunction with a public-key (asymmetric) algorithm, the key is encrypted using the public key
5836   of the receiver and subsequently decrypted using the receiver’s private key.
5837   When used in conjunction with a symmetric algorithm, the key is encrypted with a key-wrapping key shared by
5838   the sending and receiving parties and decrypted using the same key.

5839   key update
5840   A function performed on a cryptographic key to compute a new key that is related to the old key and is used to
5841   replace that key.
5842            Note: This recommendation disallows this method of replacing a key.




                                                             180


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                            Recommendation for Key Management
       December 2025                                                                                Part 1 — General

5843   key wrapping
5844   A method of cryptographically protecting keys that provides both confidentiality and integrity protection.

5845   key-wrapping key
5846   A key that is used to provide both confidentiality and integrity protection for other keys. Also see key-encryption
5847   key.

5848   keying material
5849   A cryptographic key and other parameters (e.g., IVs or algorithm parameters) used with a cryptographic
5850   algorithm.

5851   manual key transport
5852   A manual (i.e., non-automated) means of transporting cryptographic keys by physically moving a device or
5853   document that contains the key or key share.

5854   master key
5855   See key-derivation key.

5856   message authentication code (MAC)
5857   A cryptographic checksum on data that uses an approved security function and a symmetric key to detect both
5858   accidental and intentional modifications of data.

5859   metadata
5860   The information associated with a key that describes its specific characteristics, constraints, acceptable uses,
5861   and ownership. Sometimes called the key’s “attributes.”

5862   NIST standards
5863   Federal Information Processing Standards (FIPS) publications and NIST recommendations.

5864   non-repudiation
5865   A service provided using a digital signature to support a determination by a third party of whether a message
5866   was actually signed by a given entity.

5867   operational phase
5868   A phase in the life cycle of a cryptographic key whereby the key is used for standard cryptographic purposes.

5869   operational storage
5870   The normal storage of operational keying material during a key’s cryptoperiod.

5871   owner (of a certificate)
5872   A human entity who is identified as the subject in a public-key certificate or is a sponsor of a non-human entity
5873   (e.g., device, application, or process) that is identified as the certificate subject.

5874   owner (of a key or key pair)
5875   For a static key pair, the entity that is associated with the public key and authorized to use the private key. For
5876   an ephemeral key pair, the entity that generated the public/private key pair. For a symmetric key, any entity
5877   that is authorized to use the key.

5878   originator
5879   An entity that initiates an information exchange or storage event.

5880   originator-usage period
5881   The period of time in the cryptoperiod of a symmetric key during which cryptographic protection may be applied
5882   to data using that key.




                                                              181


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                              Recommendation for Key Management
       December 2025                                                                                  Part 1 — General

5883   password
5884   A string of characters (i.e., letters, numbers, and other symbols) that are used to authenticate an identity, verify
5885   access authorization, or derive cryptographic keys.

5886   period of protection
5887   The period of time during which the integrity or confidentiality of a key needs to be maintained.

5888   plaintext
5889   Intelligible data that has meaning and can be understood without the application of decryption.

5890   pre-activation state
5891   A key state in which the key has been generated but is not yet authorized for use.

5892   private key
5893   A cryptographic key used with a public-key cryptographic algorithm that is uniquely associated with an entity
5894   and is not made public. In an asymmetric-key (public-key) cryptosystem, the private key has a corresponding
5895   public key.

5896   proof-of-possession (POP)
5897   A verification process whereby assurance is obtained that the owner of a key pair actually has the private key
5898   associated with the public key.

5899   pseudorandom number generator
5900   See deterministic random bit generator (DRBG).

5901   public key
5902   A cryptographic key used with a public-key cryptographic algorithm that is uniquely associated with an entity
5903   and that may be made public. In an asymmetric-key (public-key) cryptosystem, the public key has a
5904   corresponding private key.

5905   public-key certificate
5906   A set of data that provides a unique identifier for the owner of the certificate, contains that entity’s public key
5907   and possibly other information, and is digitally signed by a trusted party (e.g., a CA), thereby binding the public
5908   key to the entity’s identifier. Additional information in the certificate could specify how the key is used and its
5909   validity period.

5910   public-key (asymmetric-key) cryptographic algorithm
5911   A cryptographic algorithm that uses two related keys: a public key and a private key. The two keys have the
5912   property that determining the private key from the public key is computationally infeasible.

5913   public-key infrastructure (PKI)
5914   A framework that is established to issue, maintain, and revoke public-key certificates.

5915   random bit generator (RBG)
5916   A device or algorithm that outputs a sequence of bits that appears to be statistically independent and unbiased.
5917   Also see random number generator.

5918   random number generator (RNG)
5919   A process used to generate an unpredictable series of numbers. Also called a random bit generator (RBG).

5920   recipient-usage period
5921   The period of time during which protected information may be processed (e.g., decrypted) using a symmetric
5922   algorithm and symmetric key.

5923   registration authority (RA)
5924   A trusted entity that establishes and vouches for the identity of a user.



                                                               182


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                            Recommendation for Key Management
       December 2025                                                                                Part 1 — General

5925   relying party
5926   A party that relies on the security and authenticity of a key or key pair to apply cryptographic protection and/or
5927   remove or verify the protection that has been applied. This includes parties that rely on the public key in a
5928   public-key certificate and parties that share a symmetric key.

5929   representative (of a key owner)
5930   See sponsor (of a key).

5931   retention period
5932   The minimum amount of time that a key or other cryptographically related information should be retained.

5933   RBG seed
5934   A string of bits that is used to initialize a DRBG. Also called a seed.

5935   secret key
5936   A single cryptographic key that is used with a symmetric-key cryptographic algorithm, is uniquely associated
5937   with one or more entities, and is not made public (i.e., the key is kept secret). A secret key is also called a
5938   symmetric key.
5939            Note: The use of the term “secret” in this context does not imply a classification level but rather implies
5940            the need to protect the key from disclosure.

5941   secret-key algorithm
5942   See symmetric-key algorithm.

5943   secret-key information
5944   The key information that needs to be kept secret (e.g., symmetric keys, private keys, key shares, and secret
5945   metadata).

5946   security categories
5947   The evaluation criteria for the security strength of an algorithm in terms of the computational resources needed
5948   to break block cipher algorithms or hash functions.

5949   secure communication protocol
5950   A communication protocol that provides the appropriate confidentiality, source authentication, and integrity
5951   protection.

5952   security domain
5953   A system or subsystem that is under the authority of a single trusted authority. Security domains may be
5954   organized (e.g., hierarchically) to form larger domains.

5955   security function
5956   Cryptographic algorithms, together with modes of operation (if appropriate); for example, block ciphers, digital
5957   signature algorithms, asymmetric key-establishment algorithms, message authentication codes, hash methods,
5958   or random bit generators. See [FIPS 140-3].

5959   security life of data
5960   The time period during which the security of the data needs to be protected (i.e., its confidentiality, integrity,
5961   or availability).

5962   security services
5963   Mechanisms used to provide confidentiality, identity authentication, integrity authentication, source
5964   authentication, and/or non-repudiation.




                                                                183


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                             Recommendation for Key Management
       December 2025                                                                                 Part 1 — General

5965   security strength
5966   A number associated with the amount of work (i.e., the number of operations) that is required to break a
5967   cryptographic algorithm or system. In this recommendation, the security strength is specified in bits and is a
5968   specific value from the set {112, 128, 192, 256}. Also called “bits of security.”

5969   seed
5970   A secret value that is used to initialize a process (e.g., initialize a DRBG) or generate keying material. Also see
5971   RBG seed.

5972   self-signed certificate
5973   A public-key certificate whose digital signature may be verified by the public key contained within the certificate.
5974   The signature on a self-signed certificate protects the integrity of the information within the certificate but does
5975   not guarantee the authenticity of that information. The trust of self-signed certificates is based on the secure
5976   procedures used to distribute them.

5977   shall
5978   This term is used to indicate a requirement of a Federal Information Processing Standards (FIPS) publication or
5979   a requirement that must be fulfilled to claim conformance to this recommendation.
5980            Note: Shall may be combined with not to become shall not.

5981   shared secret
5982   A secret value that has been computed using a key-agreement scheme and is used as input to a key-derivation
5983   method. A shared secret from a key-agreement scheme cannot be used directly as a key.

5984   shared secret key
5985   A shared secret that can be used directly as a cryptographic key in symmetric-key cryptography. It does not
5986   require additional key derivation.

5987   should
5988   This term is used to indicate a very important recommendation. Ignoring the recommendation could result in
5989   undesirable results.
5990            Note: Should may be combined with not to become should not.

5991   signature generation
5992   The use of a digital signature algorithm and a private key to generate a digital signature on data.

5993   signature verification
5994   The use of a digital signature algorithm and a public key to verify a digital signature on data. The digital signature
5995   was generated using the corresponding private key.

5996   source authentication
5997   The process of providing assurance about the source of information. Sometimes called “origin authentication.”
5998   Compare with identity authentication.

5999   split-knowledge procedure
6000   A procedure for establishing a key from multiple key shares, each of which is only known by a single entity. The
6001   key shares are combined using an approved method. See key share.

6002   sponsor (of a certificate)
6003   A human entity who is responsible for managing a certificate for the non-human entity identified as the subject
6004   in the certificate (e.g., a device, application, process). Certificate management includes applying for the
6005   certificate, generating the key pair, replacing the certificate when required, and revoking the certificate. A
6006   certificate sponsor is also a sponsor of the public key in the certificate and the corresponding private key.




                                                               184


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                             Recommendation for Key Management
       December 2025                                                                                 Part 1 — General

6007   sponsor (of a key)
6008   A human entity who is responsible for managing a key for the non-human entity (e.g., organization, device,
6009   application, or process) that is authorized to use the key.

6010   static key
6011   A key that is intended for use for a relatively long period of time and is typically intended for use in many
6012   instances of a cryptographic key-establishment scheme. Contrast with an ephemeral key.

6013   suspended state
6014   A key state in which the use of a key or key pair may be suspended for a period of time.

6015   symmetric key
6016   A single cryptographic key that is used with a symmetric-key cryptographic algorithm, is uniquely associated
6017   with one or more entities, and is not made public (i.e., the key is kept secret). A symmetric key is often called a
6018   secret key. See secret key.

6019   symmetric-key algorithm
6020   A cryptographic algorithm that uses the same secret key for an operation and its complement (e.g., encryption
6021   and decryption). Also called a secret-key algorithm.

6022   system
6023   A discrete set of resources that are organized for the collection, processing, maintenance, use, sharing,
6024   dissemination, or disposition of information.

6025   system initialization
6026   A function in the life cycle of a cryptographic key. Setting up and configuring a system for secure operation.

6027   trust anchor
6028   1. An authoritative entity for which trust is assumed. In a PKI, a trust anchor is a certification authority, which is
6029      represented by a certificate that is used to verify the signature on a certificate issued by that trust-anchor.
6030      The security of the validation process depends on the authenticity and integrity of the trust anchor’s
6031      certificate. Trust anchor certificates are often distributed as self-signed certificates.
6032   2. The self-signed public key certificate of a trusted CA.

6033   trusted channel
6034   A trusted and safe communication link established between a cryptographic module and a sender or receiver
6035   to securely communicate unprotected plaintext, critical security parameters, key components, and
6036   authentication data.

6037   unauthorized disclosure
6038   An event that involves the exposure of information to entities that are not authorized to access the information.

6039   user
6040   An individual (person). Also see entity.

6041   X.509 certificate
6042   The X.509 public-key certificate or the X.509 attribute certificate, as defined by the ISO/ITU-T X.509 standard.
6043   Most commonly (including in this document), an X.509 certificate refers to the X.509 public-key certificate.




                                                               185


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)             Recommendation for Key Management
       December 2025                                                                 Part 1 — General

6044     Appendix F. Change Log
6045   In 2025, the following changes were made to SP 800-57, Part 1:
6046       1. The NIST template for this publication was changed, resulting in moving the acronyms
6047          and definitions (in Revision 5) from Section 2 to the appendices.
6048       2. Ascon, as specified in SP 800-232, and the new quantum-resistant algorithms
6049          specified in FIPS 203, 204, and 205 have been included.
6050       3. Section 2.6: Text was added about destroying keying material.
6051       4. Section 2.7: The description of key transport has been modified slightly, and a
6052          description of key encapsulation has been added.
6053       5. Section 3.1 (previously Section 4.1): The eXtendable-Output Functions (XOFs)
6054          specified in FIPS 202 have been included, and the term “hash method” is used to refer
6055          to both methods of hashing.
6056       6. Section 3.3 (previously Section 4.3): The discussion on asymmetric-key algorithms was
6057          expanded.
6058       7. Section 4.1.1 (old Section 5.1.1): Keys used for both key-establishment and key
6059          storage are now discussed separately. The encapsulation and decapsulation keys used
6060          in FIPS 203 have been included for both automated key-establishment and key-
6061          storage applications.
6062       8. Section 4.3.4 (previously Section 5.3.4): An example for encapsulation/decapsulation
6063          keys was added.
6064       9. Section 4.3.6 (previously Section 5.3.6): Keys used for key establishment and key
6065          storage are now discussed separately. The encapsulation and decapsulation keys used
6066          in FIPS 203 have been included for both automated key-establishment and key-
6067          storage applications.
6068       10. Section 4.6.1 (previously Section 5.6.1): The security categories used in the PQC
6069           competition have been included, along with the quantum-resistant algorithms (in
6070           Table 5). TDEA is now shown as disallowed, and Ascon-AEAD128 has been added (in
6071           Table 3). DSA is shown as disallowed (in Table 4). ML-KEM, ML-DSA, SKH-DSA, and the
6072           hash-based signature algorithms in SP 800-208 have been added to Table 5. Table 6
6073           now includes Ascon-hash256, Ascon-XOF128, Ascon-CXOF128, SHAKE128, and SHAKE
6074           256.
6075       11. Section 4.6.2 (previously Section 5.6.2): The examples have been changed to use ML-
6076           KEM and ML-DSA.
6077       12. Section 4.6.3 (previously Section 5.6.3): The time frames have been removed and
6078           replaced with references to SP 800-131A.
6079       13. Section 4.6.4 (previously Section 5.6.4): This section has been updated to include
6080           additional guidance that was developed during the PQC transition and references to
6081           NIST’s crypt agility project.


                                                        186


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)                Recommendation for Key Management
       December 2025                                                                    Part 1 — General

6082       14. Section 4.6.5 (previously Section 5.6.5): New text has been included about
6083           terminating the cryptoperiod when an algorithm is disallowed in SP 800-131A.
6084       15. Section 5.1.1 (previously Section 6.1.1): The encapsulation and decapsulation keys
6085           have been added, and communication and storage applications have been separated
6086           in the table (some changes were made to the last column of the table).
6087       16. Section 5.1.2 (previously Section 6.1.2): Changes have been made to the rightmost
6088           column in the table to include seeds used for (re)generating keys (e.g., ML-KEM key
6089           pairs).
6090       17. Section 5.2.2 (previously Section 6.2.2): A subsection on key storage media (e.g., using
6091           HSMs and TPMs) has been added as Section 5.2.2.7.
6092       18. Section 5.2.3 (previously Section 6.2.2): The seed used to (re)generate a key or key
6093           pair has been added to the list of possible metadata.
6094       19. Section 5.2.2.7 (new): A section has been added to discuss keying material storage
6095           and mechanisms.
6096       20. Section 6 (previously Section 7): Encapsulation and decapsulation keys have been
6097           added. The deactivation state has been removed. A table has been added to the
6098           activation state to clarify transitions to the destroyed state (Section 6.2). Transitions
6099           from the compromised state to the destroyed state have been expanded for clarity
6100           (Section 6.3).
6101       21. Section 7 (previously Section 8): The discussion of particular keys for each key state
6102           have been removed, leaving basic descriptions of the key states.
6103       22. Section 7.1.5.2.3 (previously Section 8.1.5.2.3): A discussion of key encapsulation is
6104           now included with the discussion of key agreement.
6105       23. Section 7.1.5.3.4 (previously Section 8.1.5.3.4): The discussion of seeds has been
6106           expanded to include seeds used for the (re)generation of keys (e.g., ML-KEM keys).
6107       24. The references have been updated, including adding references to FIPS 203, FIPS 204,
6108           FIPS 205, SP 800-208, ISO/IEC 19790, NIST IR 7977, SP 800-224, SP 800-227, and SP
6109           800-232.
6110       25. Appendix B was reorganized, placing the discussions about the key types at the end
6111           of the appendix and discussing the key types in the same order as used in the main
6112           body (e.g., in Section 4.1.1). Guidance for encapsulation/decapsulation key pairs was
6113           added as well as additional and/or clarifying guidance for many of the key types.
6114       26. Appendix C was added to address uncertainties in estimating the security strengths of
6115           post-quantum cryptosystems. The categorization approach is based on the range of
6116           security strengths offered by existing NIST standards in symmetric cryptography.
6117       27. Appendix D (previously Section 2.2): New terms were added, including ML-DSA, ML-
6118           KEM, PQC, SLH-DSA, and XOF.




                                                        187


---

       NIST SP 800-57pt1r6 ipd (Initial Public Draft)             Recommendation for Key Management
       December 2025                                                                 Part 1 — General

6119       28. Appendix E (previously Section 2.1): New definitions have been added, including
6120           “destroy (a key or other data),” “eXtendable-Output Function,” “hashing method,”
6121           “security categories”, “shared secret key,” “split-knowledge procedure,” and “trusted
6122           channel.” Modified definitions now include “assurance of validity,” “decapsulation,”
6123           “encapsulation,” “contingency planning,” “key-encapsulation mechanism,” “key
6124           wrapping,” “key-wrapping key,” “private key,” “public key,” “public key certificate,”
6125           “recipient-usage period,” “security strength,” and “seed.” The definition for “domain
6126           parameter” has been changed to “algorithm parameter.”




                                                        188


---

