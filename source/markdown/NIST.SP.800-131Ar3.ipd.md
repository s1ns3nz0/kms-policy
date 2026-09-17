                  NIST Special Publication 800
                      NIST SP 800-131Ar3 ipd


Transitioning the Use of Cryptographic
           Algorithms and Key Lengths
                                          Initial Public Draft


                                                   Elaine Barker
                                                  Allen Roginsky

                  This publication is available free of charge from:
                 https://doi.org/10.6028/NIST.SP.800-131Ar3.ipd


---

                                       NIST Special Publication 800
                                           NIST SP 800-131Ar3 ipd

Transitioning the Use of Cryptographic
           Algorithms and Key Lengths
                                                                       Initial Public Draft


                                                                                 Elaine Barker
                                                                                Allen Roginsky
                                                                Computer Security Division
                                                        Information Technology Laboratory




                                       This publication is available free of charge from:
                                      https://doi.org/10.6028/NIST.SP.800-131Ar3.ipd




                                                                                    October 2024




                                                                        U.S. Department of Commerce
                                                                         Gina M. Raimondo, Secretary

                                                         National Institute of Standards and Technology
       Laurie E. Locascio, NIST Director and Under Secretary of Commerce for Standards and Technology


---

NIST SP 800-131Ar3 ipd (Initial Public Draft)                                 Transitioning the Use of Cryptographic
October 2024                                                                             Algorithms and Key Lengths


Certain commercial entities, equipment, or materials may be identified in this document in order to describe an
experimental procedure or concept adequately. Such identification is not intended to imply recommendation or
endorsement by the National Institute of Standards and Technology (NIST), nor is it intended to imply that the
entities, materials, or equipment are necessarily the best available for the purpose.

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
Copyright, Fair Use, and Licensing Statements
NIST Technical Series Publication Identifier Syntax


Publication History

Approved by the NIST Editorial Review Board on YYYY-MM-DD [will be added in final published version]


How to Cite this NIST Technical Series Publication:
Barker EB, Roginsky AL (2024) Transitioning the Use of Cryptographic Algorithms and Key Lengths. (National
Institute of Standards and Technology, Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-131Ar3 ipd.
https://doi.org/10.6028/NIST.SP.800-131Ar3.ipd


Author ORCID iDs
Elaine Barker: 0000-0003-0454-0461
Allen Roginsky: 0000-0003-2684-6736


---

NIST SP 800-131Ar3 ipd (Initial Public Draft)                            Transitioning the Use of Cryptographic
October 2024                                                                        Algorithms and Key Lengths

Public Comment Period
October 21, 2024 – December 4, 2024


Submit Comments
sp800-131a_comments@nist.gov

National Institute of Standards and Technology
Attn: Computer Security Division, Information Technology Laboratory
100 Bureau Drive (Mail Stop 8930) Gaithersburg, MD 20899-8930


Additional Information
Additional information about this publication is available at https://csrc.nist.gov/pubs/sp/800/131/a/r3/ipd,
including related content, potential updates, and document history.


All comments are subject to release under the Freedom of Information Act (FOIA).


---

     NIST SP 800-131Ar3 ipd (Initial Public Draft)                    Transitioning the Use of Cryptographic
     October 2024                                                                Algorithms and Key Lengths



 1   Abstract
 2   NIST provides cryptographic key management guidance for defining and implementing
 3   appropriate key-management procedures, using algorithms that adequately protect sensitive
 4   information, and planning for possible changes in the use of cryptography because of algorithm
 5   breaks or the availability of more powerful computing techniques. This publication provides
 6   guidance for transitions to the use of stronger cryptographic keys and more robust algorithms.

 7   Keywords
 8   cryptographic algorithm; digital signature; elliptic curves; encryption; entropy; extendable output
 9   functions; hash function; key agreement; key-derivation functions; key encapsulation; key
10   transport; key wrapping; message authentication codes; quantum-resistant algorithms; random
11   bit generation; security strength; transition.

12   Reports on Computer Systems Technology
13   The Information Technology Laboratory (ITL) at the National Institute of Standards and
14   Technology (NIST) promotes the U.S. economy and public welfare by providing technical
15   leadership for the Nation’s measurement and standards infrastructure. ITL develops tests, test
16   methods, reference data, proof of concept implementations, and technical analyses to advance
17   the development and productive use of information technology. ITL’s responsibilities include the
18   development of management, administrative, technical, and physical standards and guidelines
19   for the cost-effective security and privacy of other than national security-related information in
20   federal information systems. The Special Publication 800-series reports on ITL’s research,
21   guidelines, and outreach efforts in information system security, and its collaborative activities
22   with industry, government, and academic organizations.




                                                      i


---

     NIST SP 800-131Ar3 ipd (Initial Public Draft)                    Transitioning the Use of Cryptographic
     October 2024                                                                Algorithms and Key Lengths

23   Note to Reviewers
24   This revision proposes a) the retirement of ECB as a confidentiality mode of operation (see Sec.
25   2.2) and the use of DSA for digital signature generation (Sec. 3) and b) a schedule for the
26   retirement of SHA-1 and the 224-bit hash functions (Sec. 3 and 10). New sections are included
27   for block cipher modes, (Sec. 2.2), key generation (Sec. 4), and extendable-output functions (Sec.
28   12). These and other changes are listed in Appendix B.3.
29       Question: Does the retirement date of December 31, 2030, for the 224-bit hash functions pose
30       an unacceptable burden on implementers or users?
31   This draft revision also discusses the transition from a security strength of 112 bits to a 128-bit
32   security strength and to quantum-resistant algorithms for digital signatures and key
33   establishment. Since NIST is simultaneously working on multiple FIPS and SPs related to the
34   quantum-resistant algorithms, this draft may include references to documents that have not yet
35   been finalized, are in the process of being revised to address the availability of the quantum-
36   resistant algorithms (e.g., SP 800-57 Part 1 and SP 800-175B), or are being developed as guidance
37   for using them (e.g., a proposed transition schedule to the PQC algorithms). It is anticipated that
38   these documents will either be finalized and/or publicly available as drafts by the end of 2024.
39   NIST is in the process of developing a schedule for a transition to the quantum-resistant
40   algorithms. SP 800-131A will then be revised to be consistent with that guidance.
41




                                                     ii


---

     NIST SP 800-131Ar3 ipd (Initial Public Draft)                     Transitioning the Use of Cryptographic
     October 2024                                                                 Algorithms and Key Lengths

42   Call for Patent Claims
43   This public review includes a call for information on essential patent claims (claims whose use
44   would be required for compliance with the guidance or requirements in this Information
45   Technology Laboratory (ITL) draft publication). Such guidance and/or requirements may be
46   directly stated in this ITL Publication or by reference to another publication. This call also
47   includes disclosure, where known, of the existence of pending U.S. or foreign patent
48   applications relating to this ITL draft publication and of any relevant unexpired U.S. or foreign
49   patents.
50   ITL may require from the patent holder, or a party authorized to make assurances on its behalf,
51   in written or electronic form, either:
52   assurance in the form of a general disclaimer to the effect that such party does not hold and
53   does not currently intend holding any essential patent claim(s); or
54   assurance that a license to such essential patent claim(s) will be made available to applicants
55   desiring to utilize the license for the purpose of complying with the guidance or requirements in
56   this ITL draft publication either:
57   under reasonable terms and conditions that are demonstrably free of any unfair discrimination;
58   or
59   without compensation and under reasonable terms and conditions that are demonstrably free
60   of any unfair discrimination.
61   Such assurance shall indicate that the patent holder (or third party authorized to make
62   assurances on its behalf) will include in any documents transferring ownership of patents
63   subject to the assurance, provisions sufficient to ensure that the commitments in the assurance
64   are binding on the transferee, and that the transferee will similarly include appropriate
65   provisions in the event of future transfers with the goal of binding each successor-in-interest.
66   The assurance shall also indicate that it is intended to be binding on successors-in-interest
67   regardless of whether such provisions are included in the relevant transfer documents.
68   Such statements should be addressed to sp800-131a_comments@nist.gov
69
70




                                                      iii


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                                                        Transitioning the Use of Cryptographic
      October 2024                                                                                                    Algorithms and Key Lengths

 71   Table of Contents
 72   1. Introduction ...................................................................................................................................1
 73
 74
 75        1.2.1. Security Strengths ........................................................................................................................ 1
 76        1.2.2. Definition of Status Approval Terms ............................................................................................ 2
 77        1.2.3. Transition Strategy from 112-Bit Security Strength ..................................................................... 3
 78   2. Data Encryption and Decryption Using Block Cipher Algorithms.......................................................5
 79
 80
 81   3. Digital Signatures ...........................................................................................................................8
 82   4. Cryptographic Key Generation ......................................................................................................13
 83   5. Random Bit Generation ................................................................................................................14
 84
 85
 86
 87   6. Key Agreement Using Diffie-Hellman and MQV .............................................................................16
 88   7. Key Agreement and Key Transport Using RSA................................................................................19
 89   8. Key Establishment Using a Key Encapsulation Mechanism (KEM) ...................................................21
 90   9. Key Derivation Methods ...............................................................................................................22
 91
 92        9.1.1. One-Step Key-Derivation Functions ........................................................................................... 22
 93        9.1.2. Two-Step Key-Derivation Procedures ........................................................................................ 23
 94
 95
 96   10. Key Wrapping.............................................................................................................................26
 97   11. Hash Functions ...........................................................................................................................28
 98   12. eXtendable-Output Functions (XOFs) ..........................................................................................30
 99   13. Message Authentication Codes (MACs) .......................................................................................31
100   References.......................................................................................................................................35
101   Appendix A. Continued Use of AES ...................................................................................................40
102   Appendix B. Change History .............................................................................................................41
103   Appendix C. List of Symbols, Abbreviations, and Acronyms ...............................................................43
104   Appendix D. Glossary .......................................................................................................................44

                                                                                iv


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                                           Transitioning the Use of Cryptographic
      October 2024                                                                                       Algorithms and Key Lengths

105   List of Tables
106   Table 1. Approval status of block cipher algorithms for encryption and decryption ..............................5
107   Table 2. Approval status of the block cipher modes of operation for AES encryption and decryption ....6
108   Table 3. Approval status of algorithms used for digital signature generation .......................................9
109   Table 4. Approval status of algorithms used for random bit generation .............................................14
110   Table 6. Approval status for SP 800-56A key agreement (DH and MQV) schemes ...............................16
111   Table 7. Approval status for the RSA-based key-agreement and key-transport schemes ..................... 19
112   Table 8. Approval status for the one-step KDFs in SP 800-56C............................................................22
113   Table 9. Approval status for the two-step KDMs in SP 800-56C ..........................................................23
114   Table 10. Approval status of the algorithms used for a key derivation function (KDF) ........................ 24
115   Table 11. Approval status of the PBKDFs ...........................................................................................25
116   Table 12. Approval status of block cipher algorithms used for key wrapping ......................................26
117   Table 13. Approval status of hash functions ......................................................................................28
118   Table 14. Approval status of eXtendable-Output Functions (XOFs) ....................................................30
119   Table 15. Approval status of MAC algorithms....................................................................................31
120

121




                                                                       v


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                  Transitioning the Use of Cryptographic
      October 2024                                                              Algorithms and Key Lengths

122   Acknowledgments
123   The authors would like to specifically acknowledge the assistance of the following colleagues in
124   developing this revision of SP 800-131A: Alex Calis, Chris Celi, Lily Chen, David Cooper, Morris
125   Dworkin, Kerry McKay, Nicky Mouha, Ray Perlman, and Andrew Regenscheid.




                                                      vi


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                    Transitioning the Use of Cryptographic
      October 2024                                                                Algorithms and Key Lengths



126   1. Introduction

127   1.1. Background and Purpose
128   At the beginning of the 21st century, the National Institute of Standards and Technology (NIST)
129   began the task of providing cryptographic key-management guidance. This guidance is intended
130   to 1) encourage the specification and implementation of appropriate key-management
131   procedures, 2) use algorithms that adequately protect sensitive information, and 3) plan for
132   possible changes in the use of cryptographic algorithms, including any migration to different
133   algorithms and key lengths. The third item addresses not only the possibility of new cryptanalysis
134   but also the increasing power of classical computing technology and the emergence of quantum
135   computers.
136   This third revision of Special Publication (SP) 800-131A is intended to provide details about the
137   transitions associated with the use of cryptography by federal agencies to protect sensitive but
138   unclassified information. The document addresses the use of algorithms and key lengths
139   specified in Federal Information Processing Standards (FIPS) and SPs. Unless otherwise specified
140   (e.g., by a revision number), the latest versions of specific FIPS and SPs are referenced in the
141   discussions.
142   SP 800-131A was originally published in January 2011 and revised in 2015 and 2019. This revision
143   updates the transition guidance provided in the 2019 version and includes 1) the retirement of
144   the ECB mode when used for confidentiality (Sec. 2.2) and DSA for digital signature generation
145   (Sec. 3), 2) a schedule for the retirement of SHA-1 and the 224-bit hash functions (Sec. 3 and 10),
146   and 3), and discussions about the planned transition from 112-bit security strength to 128-bit
147   security strength and/or the use of quantum-resistant algorithms. New sections have been
148   included for block cipher modes (Sec. 2.2), key generation (Sec. 4), and extendable-output
149   functions (XOFs) (Sec. 12). These and other changes are listed in Appendix B.

150   1.2. Useful Terms for Understanding this Recommendation

151   1.2.1. Security Strengths
152   NIST Special Publication (SP) 800-57 Part 1 [SP 800-57] includes the definition of an estimated
153   maximum security strength (hereafter shortened to “security strength”) and the association of
154   the algorithms and key lengths with these security strengths. The length of the cryptographic
155   keys is an integral part of these determinations.
156   In [SP 800-57], the classical security strength provided by an algorithm with a particular key
157   length 1 is measured in bits and based on the difficulty of subverting the cryptographic protection
158   that is provided by the algorithm and key. An estimated security strength for each algorithm is


      1 The term “key size” is commonly used in other documents.




                                                                   1


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                                                       Transitioning the Use of Cryptographic
      October 2024                                                                                                   Algorithms and Key Lengths

159   provided in [SP 800-57] and the FIPS 140 Implementation Guide [FIPS 140 IG] Annex D.B. This is
160   the security strength that an algorithm with a particular key length can provide, given that the
161   key used with that algorithm is correctly generated. 2
162   The appropriate (classical) security strength to protect data depends on its sensitivity and needs
163   to be determined by the data owner (e.g., a person or organization). For the Federal Government,
164   a security strength of at least 112 bits is currently required for applying cryptographic protection
165   (e.g., for encrypting or signing data). Section 1.2.3 discusses the proposed strategy used in this
166   document for a transition from the 112-bit security strength.

167   1.2.2. Definition of Status Approval Terms
168   The terms “acceptable,” “deprecated, “disallowed,” and “legacy use” are used throughout this
169   recommendation to indicate the approval status of an algorithm. Often, the approval status for
170   an algorithm will also depend on the length and/or strength of its key, any domain parameters,
171   and the mode or manner in which it is used.
172        •     Acceptable means that the algorithm and key length/strength in a FIPS or SP are
173              approved for use in accordance with any associated guidance. The FIPS 140
174              Implementation Guide [FIPS_140_IG] may indicate additional acceptable algorithms that
175              are allowed for use but are not specified in a FIPS or NIST recommendation.
176        •     Deprecated means that the algorithm and key length/strength may be used, but there is
177              some security risk. The data owner must examine this risk potential and decide whether
178              to use a deprecated algorithm or key length.
179        •     Disallowed means that the algorithm, key length/strength, parameter set, or scheme is
180              no longer allowed for the stated purpose.
181        •     Legacy use means that the algorithm, key length/strength, parameter set, or scheme may
182              only be used to process already protected information (e.g., to decrypt ciphertext data or
183              to verify a digital signature). By default, applications should treat data processed in this
184              way as having no more assurance of integrity and/or confidentiality than unprotected
185              data. User interfaces should clearly distinguish between data processed via legacy-use
186              cryptography and data processed using cryptography that remains acceptable.
187                     The relevant risks associated with legacy use differ depending on the
188                     type of cryptography. The risk of a loss of confidentiality due to the use
189                     of weak encryption exists whether an authorized user has decrypted the
190                     data or not. Therefore, restricting the application of legacy-use
191                     decryption is not an effective risk management strategy. Instead, risk
192                     management should rely on informing the user that a loss of


      2 The term “security strength” refers to the classical security strength — a measure of the difficulty of subverting the cryptographic protection

      (e.g., discovering the key) using classical computers. For a discussion of quantum security strength (i.e., the difficulty of subverting the protection
      using quantum computers), see [NIST IR 8413].


                                                                                2


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                                                 Transitioning the Use of Cryptographic
      October 2024                                                                                             Algorithms and Key Lengths

193                    confidentiality has occurred and either revoking or replacing any secrets
194                    that may have suffered a loss of confidentiality.
195                    In contrast, the primary risk for signatures is that an authorization
196                    decision may be made based on trusting the result of verifying a weak
197                    signature. This can be mitigated by treating the signature as invalid,
198                    regardless of the result of any verification. In most cases, applications
199                    should not trust a signature verified via legacy-use cryptography without
200                    displaying a warning message.
201                    A possible case where the result of legacy-use verification may be trusted
202                    is when obtaining assurance (e.g., via local log files) that the signature
203                    was not altered before the status of the verification algorithm changed
204                    to legacy use. Even in this case, the level of risk incurred depends
205                    significantly on the level of assurance provided by cryptographic and non-
206                    cryptographic protections on the relevant log files.
207   The use of algorithms and key lengths/strengths for which the terms deprecated and legacy use
208   are listed involve some risk that increases over time.3 If it is determined that the risk is
209   unacceptable for a given application, then the algorithm or key length/strength shall be
210   considered disallowed for that application. The level of risk that can be tolerated for an
211   application and its associated data must be determined, and methods for mitigating those risks
212   must be defined.
213   This document uses the terms acceptable, deprecated, and disallowed as the approval status for
214   applying cryptographic protection (e.g., encrypting data or generating a MAC or digital signature).
215   The terms acceptable and legacy use are used as the approval status for processing already
216   protected information (e.g., decryption or MAC or digital signature verification). When
217   acceptable or deprecated is used as the status for applying protection, acceptable is used for
218   processing already protected information. When disallowed is used for the status of applying
219   protection, the legacy use status applies to the processing of already protected information.

220   1.2.3. Transition Strategy from the 112-Bit Security Strength
221   NIST recognizes that large-scale quantum computers, when available, will threaten the security
222   of several NIST-approved public-key algorithms. In particular, NIST-approved digital signature
223   schemes, key agreements using Diffie-Hellman and MQV, and key agreements and key transport
224   using RSA will need to be replaced with secure quantum-resistant counterparts. NIST has finalized
225   the initial quantum-resistant standards: [FIPS 203], [FIPS 204], and [FIPS 205]. Additional
226   standards are expected in the future. NIST encourages implementers to plan for cryptographic
227   agility to facilitate transitions to quantum-resistant algorithms where needed. Information on the
228   post-quantum project is available at https://csrc.nist.gov/projects/post-quantum-cryptography.


      3 For example, a signature that was purportedly created when the algorithm was deemed acceptable is verified after the algorithm is declared

      to only be allowed for legacy use, and the actual time when the signature was generated cannot be verified.


                                                                            3


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                     Transitioning the Use of Cryptographic
      October 2024                                                                 Algorithms and Key Lengths

229   For several years, the plan has been to transition from the 112-bit security strength to a 128-bit
230   security strength on January 1, 2031. However, since quantum-resistant digital signature and key-
231   encapsulation mechanisms are now standardized, this revision of SP 800-131A is modifying the
232   transition schedule as follows:
233       •    Transition to the 128-bit security strength for block ciphers and hash functions (for
234            collision resistance) on January 1, 2031, as planned. TDEA is disallowed as of 2024 (see
235            Sec. 2). This revision of SP 800-131A also deprecates SHA-1 and the 224-bit hash functions
236            through December 31, 2030, and disallows them thereafter for applying cryptographic
237            protection (see Sec. 11).
238       •    Deprecate the use of the 112-bit security strength for the classical digital signature and
239            key-establishment mechanisms after December 31, 2030 (rather than requiring a
240            transition to the 128-bit security strength). Instead of a two-step transition from a 112-
241            bit security strength to a 128-bit security strength and ultimately to the approved
242            quantum-resistant algorithms, this revision is proposing a one-step approach whereby
243            the quantum-resistant algorithms are implemented and available as soon as feasible.
244            Currently, a 112-bit security strength for the classical digital signature and key-
245            establishment algorithms does not appear to be in imminent danger of becoming
246            insecure in the near future, so this approach should allow an orderly transition to
247            quantum-resistant algorithms without unnecessary effort for the cryptographic
248            community.
249       •    NIST is developing a schedule for transitioning to the quantum-resistant algorithms
250            discussed in Sec. 3, 6, and 7.
251   If attacks against 112-bit security strength for digital signature and key establishment become
252   viable, a transition to the 128-bit security strength will be required. Prudent implementers and
253   users should consider transitioning to the 128-bit security strength as originally planned.




                                                       4


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                                               Transitioning the Use of Cryptographic
      October 2024                                                                                           Algorithms and Key Lengths

254   2. Data Encryption and Decryption Using Block Cipher Algorithms
255   Encryption is a cryptographic operation that is used to provide confidentiality for sensitive
256   information, and decryption is the inverse operation. Encryption and decryption using block
257   cipher algorithms employ a cryptographic primitive algorithm with a mode of operation. This
258   section addresses the encryption and decryption of data using block cipher algorithms (Sec. 2.1)
259   and the modes of operation that may be used to provide confidentiality for that data (Sec. 2.2).
260   Some of the modes may also provide data authentication.

261   2.1. Block Cipher Cryptographic Primitive Algorithms
262   Since 2004, two block cipher primitive algorithms have been approved for use by the Federal
263   Government for unclassified applications:
264        •     AES is specified in FIPS 197, Advanced Encryption Standard (AES) [FIPS 197], and has three
265              key lengths/strengths: 128, 192, and 256 bits.
266        •     The Triple Data Encryption Algorithm (TDEA) (often referred to as “Triple DES”) is
267              specified in SP 800-67r2, Recommendation for the Triple Data Encryption Algorithm
268              (TDEA) Block Cipher [SP 800-67], and has two variants known as two-key TDEA and three-
269              key TDEA. Three-key TDEA is the stronger of the two variants.
270   Table 1 provides the approval status of the block cipher primitive algorithms. These algorithms
271   are also used for purposes other than data encryption and decryption (see Sec. 5, 8, 9, and 12).
272                        Table 1. Approval status of block cipher algorithms for encryption and decryption

                                         Algorithm                                                          Status
             TDEA Encryption                                                          Disallowed
             TDEA Decryption                                                          Legacy use
             AES-128 Encryption and Decryption                                        Acceptable
             AES-192 Encryption and Decryption                                        Acceptable
             AES-256 Encryption and Decryption                                        Acceptable

273   TDEA:
274        •     Encryption using TDEA is disallowed.
275        •     Decryption using TDEA is allowed for legacy use.
276   AES:
277        Encryption and decryption using AES with 128, 192, and 256-bit keys are acceptable. 4




      4 Even with the impact of quantum computers, AES-128, AES-192, and AES-256 will remain secure for the foreseeable future. See Appendix A

      for further discussion.


                                                                          5


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                              Transitioning the Use of Cryptographic
      October 2024                                                                          Algorithms and Key Lengths

278   2.2. Block Cipher Modes of Operation for Encryption and Decryption
279   Encryption and decryption using block cipher algorithms require the use of modes of operation
280   to perform successive encryption or decryption processes on data, which in turn require multiple
281   calls to the primitive algorithm. [SP 800-38A] and a separately published addendum [SP 800-
282   38A_addendum] specify modes that are only used to perform encryption and decryption on the
283   input data. [SP 800-38C] and [SP 800-38D] specify authenticated encryption modes and are used
284   to both encrypt/decrypt data and provide a method for determining the authenticity of data
285   processed by the mode. [SP 800-38E] specifies the encryption and decryption of data for storage
286   devices with fixed-length data units. [SP 800-38G] specifies the use of encryption and decryption
287   that preserve the format of the original unencrypted data.
288   The approval status of these modes for block-cipher encryption and decryption is provided in
289   Table 2.
290                Table 2. Approval status of the block cipher modes of operation for AES encryption and
291                                                       decryption

                  Publication          Mode                                 Status
                                                                 Disallowed for data encryption
                                        ECB
                                                                    Legacy use for decryption
                                        CBC                                Acceptable
                  SP 800-38A            CFB                               Acceptable
                                        CTR                               Acceptable
                                        OFB                               Acceptable
                                      CBC-CS1
                  SP 800-38A
                                      CBC-CS2          Acceptable (will be incorporated into SP 800-38A)
                  Addendum
                                      CBC-CS3
                  SP 800-38C           CCM                              Acceptable
                  SP 800-38D           GCM                              Acceptable
                  SP 800-38E          XTS-AES                           Acceptable
                                                       Acceptable (domain size of at least one million)
                                        FF1
                  SP 800-38G                           Disallowed (domain size of less than one million)
                                        FF3                             Disallowed

292   [SP 800-38A] includes modes of encryption/decryption for use with AES and TDEA (see Sec. 2.1
293   for the approval status of these algorithms):
294       The ECB mode is disallowed for encrypting secret data but is allowed for legacy use (i.e., to
295       decrypt data that has been encrypted prior to the publication of this revision of SP 800-131A).
296       However, NIST Internal Report (IR ) 8459 discusses applications for which the ECB mode
297       remains acceptable for non-confidentiality purposes (i.e., challenge-response protocols and
298       initialization vector [IV] generation) [NIST IR 8459].
299       CBC (including CBC-CS1, CBC-CS2, and CBC-CS3, defined in [SP 800-38A_addendum]), CFB,
300       CTR, and OFB are acceptable.



                                                             6


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                     Transitioning the Use of Cryptographic
      October 2024                                                                 Algorithms and Key Lengths

301   [SP 800-38C], [SP 800-38D], and [SP 800-38E] specify, correspondingly, the CCM, GCM, and XTS-
302   AES modes for block ciphers with a block size of 128 bits (i.e., using AES):
303        The use of the CCM, GCM, and XTS-AES modes is acceptable when used as specified in SP
304        800-38C, SP 800-38D, and SP 800-38E, respectively. In addition, an implementation of the
305        AES GCM mode shall comply with one of the scenarios defined in the FIPS 140
306        Implementation Guide, Annex C.H [FIPS 140_IG].
307   [SP 800-38G] specifies two modes for format-preserving encryption and decryption:
308        1. FF1: The FF1 mode is acceptable when used as specified in SP 800-38G with the additional
309           restriction that the domain size be at least one million. The use of FF1 with a domain size
310           of less than one million is disallowed.
311        2. FF3: The use of FF3, as currently specified in SP 800-38G, is disallowed.5
312




      5 SP 800-38G will be revised to remove FF3.




                                                       7


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                                                     Transitioning the Use of Cryptographic
      October 2024                                                                                                 Algorithms and Key Lengths

313   3. Digital Signatures
314   Digital signatures provide assurance of origin authentication and data integrity. These assurances
315   can be extended to provide assurance that the signatory cannot effectively deny signing a
316   document, which is commonly known as non-repudiation. The digital signature algorithms are
317   specified in [FIPS 186], [FIPS 204], [FIPS 205], and [SP 800-208].
318   The security strength estimated for a digital signature algorithm depends on the hash function
319   used, the key length/strength and method used for key generation, and any other parameters
320   used during the digital signature process.
321        •     DSA: DSA keys are generated and used with domain parameters p, q, and g. The security
322              strength that the algorithm can provide depends on the bit lengths of p (L) and q (N) and
323              the proper generation of the domain parameters used. The specification for DSA is not
324              included in the current version of FIPS 186 (i.e., [FIPS 186-5]). However, DSA is specified
325              in the previous version (i.e., [FIPS 186-4]).
326        •     Elliptic Curve-based Digital Signatures (ECDSA and EdDSA): Keys are generated and used
327              with respect to domain parameters that define elliptic curves. The length of n (i.e., the
328              domain parameter that specifies the order of the base point G) is used to determine the
329              security strength that can be provided by a properly generated key. ECDSA and EdDSA are
330              specified in FIPS 186.
331        •     RSA: RSA keys are generated with respect to a modulus n, which is used to determine the
332              security strength that can be provided by a digital signature. The RSA algorithm for digital
333              signatures is specified in [RFC 8017], and guidance for use is provided in FIPS 186.
334        •     ML-DSA: ML-DSA is a lattice-based quantum-resistant digital signature algorithm that is
335              specified in [FIPS 204].
336        •     SLH-DSA: SLH-DSA is a quantum-resistant stateless hash-based digital signature algorithm
337              that is specified in [FIPS 205].
338        •     Stateful hash-based signatures: The LMS, HSS, XMSS, and XMSSMT quantum-resistant
339              digital signature algorithms are specified in [SP 800-208].
340   The security strength provided by a digital signature generation process is no greater than the
341   minimum of 1) the security strength that the digital signature algorithm can support with a given
342   parameter set (including the length of the key) and 2) the security strength supported by the
343   cryptographic hash method 6 that is used. See [SP 800-57] for the estimated security strength for
344   a given algorithm and parameter set.
345   Sections 11 and 12 discuss the hash methods used during the digital signature generation and
346   verification processes: hash functions and extendable-output functions (XOFs).
347   Table 3 provides the approval status of the algorithms and key lengths used for the generation
348   and verification of digital signatures in accordance with [FIPS 186], [FIPS 204], [FIPS 205], and [SP

      6 A hash method is 1) a hash function specified in either FIPS 180 or FIPS 202 or 2) an XOF specified in FIPS 202.




                                                                               8


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                                 Transitioning the Use of Cryptographic
      October 2024                                                                             Algorithms and Key Lengths

349   800-208]. The approval status of DSA, ECDSA, EdDSA, and RSA will be impacted by the transition
350   to quantum-resistant digital signature algorithms. This is indicated in the table using an asterisk
351   (*).
352                         Table 3. Approval status of algorithms used for digital signature generation

        Digital Signature
                                                 Criteria                                      Status
           Algorithm

         DSA generation                                                                      Disallowed
                                          All security strengths
         DSA verification                                                                    Legacy use

                                      < 112 bits of security strength                        Disallowed

                                                                                      Acceptable through 2030
        ECDSA generation         ≥ 112 but < 128 bits of security strength
                                                                                      Deprecated after 2030*

                                      ≥ 128 bits of security strength                       Acceptable*

                                      < 112 bits of security strength                        Legacy use
       ECDSA verification
                                      ≥ 112 bits of security strength                       Acceptable*

        EdDSA generation
                                                                                            Acceptable*
         and verification             ≥ 128 bits of security strength

                                      < 112 bits of security strength                        Disallowed

         RSA generation                                                               Acceptable through 2030
         (PKCS #1 v1.5 &         ≥ 112 but < 128 bits of security strength
                                                                                      Deprecated after 2030*
              PSS)

                                      ≥ 128 bits of security strength                       Acceptable*

         RSA verification             < 112 bits of security strength                        Legacy use
         (PKCS #1 v1.5 &
              PSS)                    ≥ 112 bits of security strength                       Acceptable*

         RSA generation
                                                                                             Disallowed
           (ASC X9.31)
                                          All security strengths
         RSA verification
                                                                                             Legacy use
           (ASC X9.31)
            ML-DSA
         generation and                Parameter sets in FIPS 204                            Acceptable
           verification
            SLH-DSA                    Parameter sets in FIPS 205                            Acceptable


                                                                   9


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                      Transitioning the Use of Cryptographic
      October 2024                                                                  Algorithms and Key Lengths

         generation and
           verification
        LMS, HSS, XMSS,
       XMSSMT generation           Parameter sets in SP 800-208                  Acceptable
         and verification


353   DSA:
354       •    Signature generation:
355                o Signature generation using DSA is disallowed.
356       •    Signature verification:
357                o Verification of DSA digital signatures is allowed for legacy use when using
358                  previously approved domain parameters and private keys.
359   ECDSA and EdDSA:
360       •    Signature generation: The security strength provided by an elliptic curve signature is 1/2
361            of the length of the domain parameter n. Recommended and deprecated elliptic curves
362            for digital signature generation are provided in [SP 800-186]. Elliptic curves that meet the
363            security strength requirements are also allowed when they satisfy the requirements of
364            FIPS 140 Implementation Guide [FIPS 140 IG], Annex C.A.
365                o ECDSA signature generation providing less than 112 bits of security is disallowed.
366                o ECDSA signature generation providing at least 112 bits of security (but less than
367                  128 bits of security) is acceptable through December 31, 2030. For these curves,
368                  224 ≤ len(n) < 256.
369                     After December 31, 2030, the use of these curves and keys is deprecated for
370                     digital signature generation.
371                o ECDSA and EdDSA signature generation providing at least 128 bits of security is
372                  acceptable. These signatures shall be generated using elliptic curves and private
373                  keys such that len(n) ≥ 256.
374       •    Signature verification:
375                o Signature verification of ECDSA digital signatures that were generated to provide
376                  less than 112 bits of security is allowed for legacy use when using curves and
377                  public keys such that 160 ≤ len(n) < 224.
378                o Signature verification of ECDSA digital signatures that were generated to provide
379                  at least 112 bits of security is acceptable using the recommended elliptic curves
380                  included in [SP 800-186]. In this case, len(n) ≥ 224.
381                o Signature verification of EdDSA digital signatures is acceptable using the
382                  recommended elliptic curves included in [SP 800-186] where len(n) ≥ 256. The use
383                  of EdDSA was never approved for a security strength less than 128 bits.

                                                           10


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                      Transitioning the Use of Cryptographic
      October 2024                                                                  Algorithms and Key Lengths

384                o Signature verification using curves that comply with FIPS 140 Implementation
385                  Guide [FIPS140_IG], Annex C.A is allowed for legacy use.
386   RSA (PKCS #1 v1.5 & PSS):
387       •    Signature generation: The security strength provided by an RSA signature depends on the
388            bit length of the modulus n. The security strength associated with several values of len(n)
389            is provided in [SP 800-57]. The security strength associated with other values of len(n)
390            may be estimated using the formula in FIPS 140 Implementation Guide [FIPS140_IG],
391            Annex D.B.
392                o Signature generation providing less than 112 bits of security is disallowed.
393                o Signature generation providing at least 112 bits of security (but less than 128 bits
394                  of security) is acceptable through December 31, 2030. These signatures shall be
395                  generated using private keys and a modulus n such that 2048 ≤ len(n) < 3072.
396                     After December 31, 2030, the use of these moduli and keys is deprecated for
397                     digital signature generation.
398                o Signature generation providing at least 128 bits of security is acceptable. These
399                  signatures shall be generated using a modulus n and public keys such that len(n)
400                  ≥ 3072.
401       •    Signature verification:
402                o Signature verification using public keys providing less than 112 bits of security is
403                  allowed for legacy use when the modulus n and the public keys are such that 1024
404                  ≤ len(n) and len(n) is a multiple of 256.
405                o Signature verification using public keys providing at least 112 of security is
406                  acceptable. Verification requires the use of a modulus n and public keys such that
407                  len(n) ≥ 2048.
408   RSA (ASC [X9.31]): Approved in FIPS 186-4.
409       •    Signature generation:
410                o Signature generation in accordance with ASC [X9.31] is disallowed.
411       •    Signature verification:
412                o Signature verification of signatures generated in accordance with ASC [X9.31] is
413                  allowed for legacy use.
414   ML-DSA:
415       •    Signature generation and verification are acceptable using the parameter sets listed in
416            [FIPS 204].
417   SLH-DSA:



                                                       11


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                   Transitioning the Use of Cryptographic
      October 2024                                                               Algorithms and Key Lengths

418       •    Signature generation and verification are acceptable using the parameter sets listed in
419            [FIPS 205].
420   LMS, HSS, XMSS, XMSSMT:
421       •    Signature generation and verification are acceptable using the parameter sets listed in
422            [SP 800-208].
423




                                                      12


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                    Transitioning the Use of Cryptographic
      October 2024                                                                Algorithms and Key Lengths

424   4. Cryptographic Key Generation
425   [SP 800-133] addresses the generation of the cryptographic keys used in cryptography. The keys
426   are either 1) generated using mathematical processing on the output of approved random bit
427   generators (RBGs) and possibly other parameters or 2) generated based on keys that are
428   generated in this fashion. These keys shall be obtained directly or indirectly from the output of
429   an approved RBG as specified in the [SP 800-90] series and generated in accordance with
430   appropriate FIPS or NIST SPs (e.g., [SP 800-108] for key derivation from a preexisting shared key).
431   [SP 800-133] includes methods for producing a key by:
432       •    Combining the output of an approved RBG with independently determined data of the
433            same length by exclusive-oring the values and
434       •    Combining independently generated keys with other (independently generated) keys
435            and/or independently determined data by concatenation, XOR-oring, or using a specified
436            key-extraction process.
437   These methods for determining keys are acceptable when consistent with the requirements of
438   the application for which the keys will be used.




                                                      13


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                               Transitioning the Use of Cryptographic
      October 2024                                                                           Algorithms and Key Lengths

439   5. Random Bit Generation
440   Random numbers are used for various purposes, such as the generation of keys, nonces, and
441   authentication challenges. The SP 800-90 series of documents specifies methods for generating
442   random bits for these purposes.

443   5.1. Deterministic Random Bit Generator Mechanisms (DRBGs)
444   Several deterministic random bit generator (DRBG) mechanisms have been specified for use by
445   the Federal Government. [SP 800-90A] includes three DRBGs: Hash_DRBG, HMAC_DRBG, and
446   CTR_DRBG, which are specified to include either a hash function or a block cipher (e.g., AES) as
447   cryptographic primitives.
448   The approval status for the DRBGs and the cryptographic primitives they use is provided in Table
449   4.
450                         Table 4. Approval status of algorithms used for random bit generation

                          DRBG                             Crypto. Primitive                    Status
                                                        SHA-1, SHA-224, SHA-           Deprecated through 2030
                                                       512/224, and SHA3-224            Disallowed after 2030
              Hash_DRBG and HMAC_DRBG
                                                      All other SHA-2 and SHA-3
                                                                                              Acceptable
                                                            hash functions
                                                                 TDEA                         Disallowed
                       CTR_DRBG
                                                      AES-128, AES-192, AES-256               Acceptable

451   Hash_DRBG and HMAC_DRBG:
452       •    The use of SHA-1 or a 224-bit hash function (i.e., SHA-224, SHA-512/224, or SHA3-224) as
453            the cryptographic primitive in Hash_DRBG and HMAC DRBG is deprecated through
454            December 31, 2030, and disallowed after 2030.
455       •    The use of Hash_DRBG and HMAC_DRBG is acceptable with any other SHA-2 or SHA-3
456            hash function specified in [FIPS 180] or [FIPS 202] (i.e., SHA-256, SHA-384, SHA-512, SHA-
457            512/256, SHA3-256, SHA3-384, and SHA3-512).
458   CTR_DRBG:
459       •    The use of CTR_DRBG using TDEA as the cryptographic primitive is disallowed.
460       •    The use of CTR_DRBG using AES-128, AES-192, or AES-256 is acceptable.

461   5.2. Entropy Sources
462   Entropy sources provide entropy for RBGs. [SP 800-90B] provides guidance for designing and
463   testing entropy sources. Additional guidance related to the validation of entropy sources is
464   provided in the FIPS 140 Implementation Guide [FIPS 140 IG], Sec. 9.3.A and Annexes D.J, D.K,
465   and D.O.


                                                              14


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                  Transitioning the Use of Cryptographic
      October 2024                                                              Algorithms and Key Lengths

466   5.3. Random Bit Generator (RBG) Constructions
467   [SP 800-90C] provides constructions for designing RBGs based on the use of entropy sources that
468   are compliant with [SP 800-90B] and the DRBGs specified in [SP 800-90A]. [SP 800-90C] is
469   currently in draft form; the RBG constructions in [SP 800-90C] are acceptable.
470




                                                      15


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                            Transitioning the Use of Cryptographic
      October 2024                                                                        Algorithms and Key Lengths

471   6. Key Agreement Using Diffie-Hellman and MQV
472   Key agreement is a technique for establishing keying material via an electronic key-agreement
473   transaction between two entities that intend to communicate. Both parties contribute
474   information so that neither party can predetermine the value of the resulting secret keying
475   material independently from the contributions of the other party. The agreed-upon keys are not
476   transmitted between the two entities.
477   [SP 800-56A] specifies two families of key-agreement schemes: Diffie-Hellman (DH) and Menezes-
478   Qu-Vanstone (MQV). Each has been defined over two different mathematical structures: finite
479   fields and elliptic curves.
480   Key agreement, as specified in [SP 800-56A], includes two steps: the use of an appropriate DH or
481   MQV “primitive” to generate a shared secret and the use of a key-derivation method (KDM) to
482   generate one or more keys from the resulting shared secret. [SP 800-56A] contains the DH and
483   MQV primitives and refers to [SP 800-56C] for KDMs. The approval status for these key-derivation
484   methods is discussed in Sec. 9.1.
485   The security strength of a key-agreement scheme specified in [SP 800-56A] depends on the key-
486   agreement algorithm, the parameters used with that algorithm (e.g., the keys), and its form (i.e.,
487   finite field or elliptic curve).
488       •    Finite field DH and MQV: The keys for these algorithms are generated and used with
489            domain parameters p, q, and g. The security strength that can be provided by the
490            algorithm depends on the length of p, the length of q, and the proper generation of the
491            domain parameters and the key.
492       •    Elliptic Curve DH and MQV: The keys for these algorithms are generated and used with
493            respect to domain parameters that define elliptic curves. The length of n (i.e., the order
494            of the base point G) is used to determine the security strength that can be provided by a
495            properly generated curve.
496   Table 6 contains the Federal Government approval status for the DH and MQV key-agreement
497   schemes. In some cases, a scheme is only allowed for legacy use; additional details below the
498   table indicate the conditions for allowing continued use (e.g., the associated dates and
499   parameters). The approval status of the schemes in this section will be impacted by the transition
500   to quantum-resistant key-establishment methods. This is indicated in the table using an asterisk
501   (*).
502                    Table 5. Approval status for SP 800-56A key agreement (DH and MQV) schemes


                   Scheme                         Domain Parameters                         Status


                                             < 112 bits of security strength              Legacy use




                                                               16


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                            Transitioning the Use of Cryptographic
      October 2024                                                                        Algorithms and Key Lengths


                   Scheme                         Domain Parameters                         Status


                                                                                   Acceptable through 2030
           SP 800-56A DH and MQV              112 bits of security strength
                                                                                   Deprecated after 2030*
        schemes using finite fields and
                elliptic curves
                                             ≥ 128 bits of security strength             Acceptable*

              Non-conformance to
                                                          Any                             Legacy use
                  SP 800-56A

503   [SP 800-56A] DH and MQV schemes using finite fields:
504       •    For key-agreement transactions providing less than 112 bits of security strength (i.e.,
505            using domain parameters where len(p) < 2048 or len(q) < 224):
506            The initiation of a key-agreement transaction providing less than 112 bits of security is
507            disallowed.
508            The processing of information in a key-agreement transaction is allowed for legacy use
509            when len(p) = 1024 or len(q) = 160. See parameter set FA in [SP 800-56Ar2].
510       •    Key-agreement transactions providing 112 bits of security strength are acceptable
511            through December 31, 2030, using the following domain parameters:
512                o The MODP-2048 safe-prime group specified in [RFC 3526] (listed in [SP 800-56A])
513                o The ffdhe2048 safe-prime group specified in [RFC 7919] (listed in [SP 800-56A])
514                o For FIPS 186-type domain parameters, (len(p), len(q)) = (2048, 224) or (2048, 256);
515                  these domain parameters are provided as parameter sets FB and FC in [SP 800-
516                  56A]
517            After December 31, 2030, the use of these domain parameters is deprecated.
518       •    Key-agreement transactions providing at least 128 bits of security strength are acceptable
519            in the following cases:
520                o The following safe-prime groups are used:
521                             The MODP-X safe-prime group specified in [RFC 3526] or
522                             The ffdheX safe-prime group specified in [RFC 7919],
523                     where X = 3072, 4096, 6144, or 8192 (listed in [SP 800-56A]).
524   [SP 800-56A] DH and MQV schemes using elliptic curves:
525       •    For key-agreement transactions providing less than 112 bits of security strength (i.e.,
526            using curves where len(n) < 224):
527            Initiating a key-agreement transaction providing less than 112 bits of security strength is
528            disallowed.

                                                                17


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                    Transitioning the Use of Cryptographic
      October 2024                                                                Algorithms and Key Lengths

529            The processing of information in a key-agreement transaction is allowed for legacy use
530            when len(n) = 160 to 223. See parameter set EA in [SP 800-56Ar2].
531       •    Key-agreement transactions providing 112 bits of security strength are acceptable
532            through December 31, 2030, using the following curves:
533                o The P-224 curve specified in [SP 800-186] (see parameter set EB in [SP 800-56A])
534                  or
535                o The brainpoolP224r1 and brainpoolP224t1 curves specified in [RFC 5639] (see FIPS
536                  140 Implementation Guide [FIPS 140_IG], Annex C.A.).
537            After December 31, 2030, the use of these curves is deprecated.
538       •    Key-agreement transactions providing at least 128 bits of security strength using the
539            following elliptic curves are acceptable:
540                o P-256, P-384, P-521, K-283, K-409, K-571, B-283, B-409, B-571, and sep256k1, as
541                  specified in [SP 800-186] (see parameter sets EC, ED, and EE in [SP 800-56A])
542                o The brainpool curves and twisted variants of these curves specified in [RFC 5639]:
543                  brainpoolP256r1,     brainpoolP320r1,     brainpoolP384r1,      brainpoolP512r1,
544                  brainpoolP256t1, brainpoolP320t1, brainpoolP384t1, and brainpoolP512t1 (see
545                  [FIPS 140 IG], Annex C.A)
546   Schemes not compliant with [SP 800-56A]:
547            Initiating a key-agreement transaction using these schemes is disallowed.
548            Processing the information in a key-agreement transaction using these schemes is
549            allowed for legacy use when using parameters that were previously acceptable.
550




                                                      18


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                                          Transitioning the Use of Cryptographic
      October 2024                                                                                      Algorithms and Key Lengths

551   7. Key Agreement and Key Transport Using RSA
552   [SP 800-56B] specifies the use of RSA for both key-agreement and key-transport transactions. Key
553   agreement is a technique in which both parties contribute information to the generation of keying
554   material so that neither party can predetermine the value of the secret keying material
555   independently from the contributions of the other party. Two key-agreement schemes are
556   specified: KAS1 and KAS2. Key transport is a key-establishment technique in which only one party
557   determines the key and sends it to the other party. One key-transport scheme is specified (i.e.,
558   RSA-OAEP), and another general hybrid method is described.
559   RSA keys are generated with respect to the desired bit length of a modulus n. The length of n is
560   used to determine the security strength of a key-establishment scheme that uses n, assuming
561   that n and the RSA keys are generated as specified in [SP 800-56B]. 7
562   [SP 800-56B] provides guidance on key lengths for RSA and explicitly specifies several key lengths
563   and the intended security strengths beginning with len(n) = 2048, which is estimated to support
564   a security strength of 112 bits. Additional key lengths greater than 2048 bits and not explicitly
565   listed in [SP 800-56B] may also be used. The approximate security strength that is supported by
566   a given key length may be estimated using a formula in [SP 800-56B] and Annex D.B of [FIPS 140
567   IG].
568   Table 7 provides the approval status for the choice of len(n). The approval status of the schemes
569   in this section will be impacted by the transition to quantum-resistant key-establishment
570   methods. This is indicated by an asterisk (*) in the table.
571                    Table 6. Approval status for the RSA-based key-agreement and key-transport schemes

                        Scheme                                   Domain Parameters                           Status
                                                           < 112 bits of security strength                 Legacy use

                                                                                                   Acceptable through 2030
         SP 800-56B Key-Agreement and                       112 bits of security strength
                                                                                                    Deprecated after 2030*
             Key-Transport schemes

                                                           ≥ 128 bits of security strength                Acceptable*

                Non-conformance to
                                                                        Any                                Legacy use
                    SP 800-56B

572   [SP 800-56B] RSA key-agreement and key-transport schemes:
573        •     For key establishment transactions providing less than 112 bits of security strength (i.e.,
574              len(n) < 2048):
575              Initiating a key-establishment transaction providing less than 112 bits of security strength
576              is disallowed.



      7 [SP 800-56B] refers to [FIPS 186] for generation guidance.




                                                                         19


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                       Transitioning the Use of Cryptographic
      October 2024                                                                   Algorithms and Key Lengths

577            The processing of information in a key-establishment transaction is allowed for legacy use
578            when len(n) = 1024 or len(q) = 160. See parameter set FA in [SP 800-56Ar2].
579       •    Key-establishment transactions that provide 112 bits of security strength (i.e., 2048 ≤
580            len(n) < 3072):
581            Key establishment is acceptable through December 31, 2030, using the schemes specified
582            in [SP 800-56B].
583            After December 31, 2030, key establishment using these values of len(n) is deprecated.
584       •    Key-establishment transactions providing at least 128 bits of security strength (i.e., len(n)
585            ≥ 3072):
586            Key establishment is acceptable using the schemes specified in [SP 800-56B].
587   Non-[SP 800-56B]-compliant RSA key-establishment schemes that were previously allowed in
588   [FIPS 140 IG]:
589       •    Initiating a key-establishment transaction using a non-[SP 800-56B]-compliant scheme is
590            disallowed.
591       •    The processing of information in a key-establishment transaction using a non-[SP 800-
592            56B]-compliant scheme is allowed for legacy use when len(n) ≥ 1024.
593




                                                        20


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                                 Transitioning the Use of Cryptographic
      October 2024                                                                             Algorithms and Key Lengths

594   8. Key Establishment Using a Key Encapsulation Mechanism (KEM)
595   [FIPS 203] specifies a quantum-resistant key encapsulation mechanism (KEM) — a set of
596   algorithms that can be used by two parties to establish a secret key over a public channel under
597   certain conditions. A key that is securely established using a KEM can then be used with
598   symmetric-key cryptographic algorithms to perform basic tasks in secure communications, such
599   as encryption and authentication.
600   [FIPS 203] specifies three parameter sets for a key-encapsulation mechanism (ML-KEM): ML-
601   KEM-512, ML-KEM-768, and ML-KEM-1024.
602   The use of any of these approved KEMs is acceptable for establishing keying material between
603   two parties.8




      8 Guidance for the use of a KEM for key establishment is under development.




                                                                          21


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                              Transitioning the Use of Cryptographic
      October 2024                                                                          Algorithms and Key Lengths

604   9. Key Derivation Methods
605   One or more keys or other keying material may be derived using pre-established key-derivation
606   keys (KDKs). A KDK may be established using a key-establishment scheme (see Sec. 6 and 7) or
607   manual key-distribution method or generated using an RBG (see Sec. 5) or a previous instance of
608   a key-derivation function.

609   9.1. Key-Derivation Methods in SP 800-56C
610   [SP 800-56C] provides key-derivation methods (KDMs) for key-establishment schemes in [SP 800-
611   56A] and [SP 800-56B] (see Sec. 6 and 7 herein, respectively). [SP 800-56C] specifies one-step
612   key-derivation functions (KDFs) and two-step key-derivation procedures. When a key-derivation
613   method is allowed for legacy use, other conditions specified in Sec. 6 and 7 for the key-
614   establishment schemes also apply. Approved key derivation methods are also provided in [SP
615   800-135] for specific applications.

616   9.1.1. One-Step Key-Derivation Functions
617   One-step key-derivation functions use a hash function, as specified in [FIPS 180] or [FIPS202];
618   HMAC, as specified in [SP 800-224]; or KMAC, as specified in [SP 800-185].
619   Table 8 provides the approval status of the one-step key-derivation functions specified in [SP 800-
620   56C].
621                              Table 7. Approval status for the one-step KDFs in SP 800-56C

                H(x)                       Crypto. Primitive                               Status

                              SHA-1, SHA-224, SHA-512/224, and SHA3-              Deprecated through 2030
           Hash function                        224                                Legacy use after 2030
               and
              HMAC                      All other hash functions                         Acceptable


               KMAC                    KMAC128 and KMAC256                               Acceptable


622   H(x) is a hash function or HMAC:
623        •    The use of SHA-1 and the 224-bit hash functions (i.e., SHA-224, SHA-512/224, or SHA3-
624             224):
625             The use of these hash functions for one-step key derivation during a key-establishment
626             transaction is deprecated through December 31, 2030.
627             After 2030, the use of these hash functions is allowed for legacy use to derive keying
628             material using the information from a key-establishment transaction (also see Sec. 6 and
629             7).


                                                               22


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                              Transitioning the Use of Cryptographic
      October 2024                                                                          Algorithms and Key Lengths

630        •    The use of all other hash functions specified in [FIPS 180] and [FIPS 202] for one-step key
631             derivation is acceptable (i.e., SHA-256, SHA-384, SHA-512, SHA-512/256, SHA3-256,
632             SHA3-384, and SHA3-512).
633   H(x) is KMAC:
634        •    The use of KMAC128 and KMAC256 for one-step key derivation is acceptable.

635   9.1.2. Two-Step Key-Derivation Procedures
636   Two-step key-derivation procedures use separate steps for randomness extraction and key
637   expansion based on HMAC, as specified in [SP 800-224], or CMAC, as specified in [SP 800-38B].
638   Table 9 provides the approval status of the two-step key-derivation methods specified in [SP 800-
639   56C].
640                              Table 8. Approval status for the two-step KDMs in SP 800-56C

            MAC Algorithm                    Crypto. Primitive                             Status

                                    SHA-1, SHA-224, SHA-512/224, and             Deprecated through 2030
                                                SHA3-224                          Legacy use after 2030
               HMAC-hash
                                          All other hash functions                      Acceptable


               AES-CMAC               AES-128, AES-192, and AES-256                     Acceptable



641   HMAC-hash:
642        •    The use of SHA-1 and the 224-bit hash functions (i.e., SHA-224, SHA-512/224, and SHA3-
643             224) for two-step key derivation using HMAC is deprecated through December 31, 2030,
644             and allowed for legacy use thereafter.
645        •    The use all other hash functions specified in [FIPS 180] and [FIPS 202] for two-step key
646             derivation using HMAC is acceptable (i.e., using SHA-256, SHA-384, SHA-512, SHA-
647             512/256, SHA3-256, SHA3-384, and SHA3-512).
648   AES-CMAC:
649       1. The use of AES-128, AES-192, or AES-256 as the cryptographic primitive for two-step key
650          derivation using CMAC is acceptable.




                                                                 23


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                               Transitioning the Use of Cryptographic
      October 2024                                                                           Algorithms and Key Lengths

651   9.2. Key-Derivation Functions in SP 800-108
652   [SP 800-108] specifies KDFs that use pseudorandom functions (PRFs) and a cryptographic key
653   (called a key-derivation key) to generate additional keys. Three PRFs are used in the KDFs
654   specified in [SP 800-108]:
655       1. HMAC, as specified in [SP 800-224], requires the use of a hash function (see Sec. 10).
656       2. CMAC, as specified in [SP 800-38B], requires the use of a block cipher algorithm (e.g., AES-
657          128, which is specified in [FIPS 197]).
658       3. KMAC, as specified in [SP 800-185].
659   HMAC, CMAC, and KMAC are also known as message authentication code (MAC) algorithms.
660   Section 13 discusses these algorithms and the keys used with them.
661   Table 10 provides the approval status of the PRFs for key derivation, as specified in [SP 800-108].
662                   Table 9. Approval status of the algorithms used for a key derivation function (KDF)

               KDF Type                         Crypto. Primitive                            Status
                                      SHA-1, SHA-224, SHA-512/224, and               Deprecated through 2030
          HMAC-based KDF                           SHA3-224                           Legacy use after 2030
                                       All other approved hash functions                   Acceptable

                                                      TDEA                                   Legacy use
          CMAC-based KDF
                                        AES-128, AES-192, and AES-256                        Acceptable
           KMAC-based KDF                  KMAC128 and KMAC 256                              Acceptable

663   HMAC-based KDF:
664       •    The use of SHA-1 and the 224-bit hash functions (i.e., SHA-224, SHA-512/224, and SHA3-
665            224) for key derivation using HMAC is deprecated through December 31, 2030, and
666            allowed for legacy use thereafter.
667       •    The use of all other hash functions specified in [FIPS 180] and [FIPS 202] for key derivation
668            using HMAC is acceptable (i.e., using SHA-256, SHA-384, SHA-512, SHA-512/256, SHA3-
669            256, SHA3-384, and SHA3-512).
670   CMAC-based KDF:
671       •    The use of TDEA, as specified in [SP 800-67], is disallowed for initiating a transaction that
672            uses a CMAC-based KDF for the key-derivation process.
673            The use of TDEA for CMAC-based key derivation is allowed for legacy use when processing
674            a transaction.
675       •    The use of AES for CMAC-based key derivation is acceptable.




                                                              24


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                              Transitioning the Use of Cryptographic
      October 2024                                                                          Algorithms and Key Lengths

676   KMAC-based KDF:
677       •    For KMAC-based KDFs, the use of KMAC128 and KMAC256 (as specified in [SP 800-185])
678            is acceptable.

679   9.3. Key-Derivation in SP 800-132
680   [SP 800-132] specifies a family of password-based key-derivation functions (PBKDFs) for deriving
681   cryptographic keys from passwords or passphrases to protect electronically stored data or data
682   protection keys. The PBKDFs require the use of a randomly generated salt of at least 128 bits, a
683   minimum iteration counter of 1000, and an HMAC with an approved hash function.
684   Table 11 provides the approval status for PBKDFs.
685                                         Table 10. Approval status of the PBKDFs

                            HMAC Crypto. Primitive                              Status
                        SHA-1, SHA-224, SHA-512/224, and                Deprecated through 2030
                                    SHA3-224                             Legacy use after 2030
                             All other hash functions                         Acceptable

686   Password-based key derivation using HMAC:
687       •    The use of SHA-1 and the 224-bit hash functions (i.e., SHA-224, SHA-512/224, and SHA3-
688            224) for password-based key derivation using HMAC is deprecated through December 31,
689            2030, and allowed for legacy use thereafter.
690       •    The use of all other hash functions specified in [FIPS 180] and [FIPS 202] for password-
691            based key derivation using HMAC is acceptable (i.e., using SHA-256, SHA-384, SHA-512,
692            SHA-512/256, SHA3-256, SHA3-384, and SHA3-512).




                                                              25


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                               Transitioning the Use of Cryptographic
      October 2024                                                                           Algorithms and Key Lengths

693   10. Key Wrapping
694   Key wrapping is the encryption and integrity protection of keying material using a block-cipher
695   key-wrapping algorithm and a symmetric key. Approved methods for key wrapping using a block
696   cipher are provided in [SP 800-38F].
697   [SP 800-38F] specifies three algorithms for key wrapping that use block ciphers: KW and KWP,
698   which use AES (as specified in [FIPS 197]); and TKW, which uses TDEA (as specified in [SP 800-
699   67]). [SP 800-38F] also approves the CCM and GCM authenticated-encryption modes specified in
700   [SP 800-38C] and [SP 800-38D] for key wrapping using AES, as well as combinations of an
701   approved encryption mode (e.g., AES-CBC) with an approved authentication method (e.g., HMAC
702   or a digital signature).
703   Table 12 provides the approval status of the block cipher algorithms that may be used for key
704   wrapping.
705                       Table 11. Approval status of block cipher algorithms used for key wrapping

                 Algorithm                                        Use                                    Status
                    TDEA                                     Key wrapping                              Disallowed
                (using TKW)                                 Key unwrapping                             Legacy use
        KW, KWP, CCM and GCM (using
                    AES)                              Key wrapping and unwrapping                      Acceptable

       The combination of an approved         Key wrapping using separate encryption and
                                                                                                       Deprecated
       encryption mode and approved                    authentication processes
        authentication method other          Key unwrapping using separate decryption and
                                                                                                       Acceptable
        than KW, KWP, CCM, or GCM             authenticity/integrity verification processes

706   TDEA (using TKW, as specified in [SP 800-38F]):
707       •    The use of TDEA for key wrapping is disallowed.
708       •    The use of TDEA for unwrapping is allowed for legacy use.
709   KW and KWP (specified in [SP 800-38F]), CCM (specified in [SP 800-38C]), and GCM (specified in
710   [SP 800-38D]):
711       •    The use of KW, KWP, CCM, and GCM using AES for both key wrapping and unwrapping is
712            acceptable.
713   The combination of an approved encryption mode and an approved authentication method
714   other than KW, KWP, CCM, and GCM:
715       •    The use of an approved encryption mode and an approved authentication method for
716            key wrapping is deprecated until additional guidance is provided for using these
717            combinations securely.
718            The approved AES encryption modes include:
719                o The CBC, CFB, OFB, and CTR modes specified in [SP 800-38A].


                                                              26


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                    Transitioning the Use of Cryptographic
      October 2024                                                                Algorithms and Key Lengths

720            The authentication methods include:
721                o The CMAC mode specified in [SP 800-38B];
722                o The GMAC mode specified in [SP 800-38D];
723                o A digital signature scheme specified in [FIPS 186], [FIPS 204], [FIPS 205], or [SP
724                  800-208] (see Sec. 3);
725                o HMAC, as specified in [SP 800-224]; and
726                o KMAC, as specified in [SP 800-185].




                                                      27


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                                 Transitioning the Use of Cryptographic
      October 2024                                                                             Algorithms and Key Lengths

727   11. Hash Functions
728   A hash function produces a condensed representation of its input by taking an input of arbitrary
729   length and outputting a value with a predetermined length. Hash functions are used in the
730   generation and verification of digital signatures, key derivation, random number generation,
731   computation of message authentication codes, and hash-only applications.
732   Several hash functions have been approved and specified:
733       •    [FIPS 180] specifies SHA-1 and the SHA-2 family of hash functions (i.e., SHA-224, SHA-256,
734            SHA-384, SHA-512, SHA-512/224, and SHA-512/256). Information about the security
735            strengths that can be provided by these hash functions is given in [SP 800-57].
736       •    [FIPS 202] specifies the SHA-3 family of hash functions (i.e., SHA3-224, SHA3-256, SHA3-
737            384, and SHA3-512). Discussions about the SHA-3 hash functions are provided, and the
738            security strengths that can be provided by these functions are given in [SP 800-57].
739       •    [SP 800-185] specifies two SHA-3-derived hash functions (i.e., TupleHash and
740            ParallelHash) that are based on the XOFs specified in [FIPS 202] and discusses their use
741            and the security strengths that they can support.
742   Table 13 provides the approval status of the hash functions.
743                                       Table 12. Approval status of hash functions

              Hash Function                             Use                                        Status

                                            Digital signature generation                        Disallowed

                                           Digital signature verification                       Legacy use
                 SHA-1
                                       Applying protection for non-digital-              Deprecated through 2030
                                             signature applications                        Disallowed after 2030
                                    Processing already protected information             Acceptable through 2030
                                     using non-digital signature applications              Legacy use after 2030
             SHA-224,                                                                    Deprecated through 2030
                                               Applying protection
       SHA-512/224, and SHA3-                                                              Disallowed after 2030
                224                                                                      Acceptable through 2030
                                    Processing already-protected information
                                                                                           Legacy use after 2030
       All other SHA-2 and SHA-                          Acceptable for all hash function applications
            3 hash functions
             TupleHash and
                                                                            Acceptable
              ParallelHash

744   SHA-1:
745       •    Digital signature generation:
746            SHA-1 is disallowed for digital signature generation.




                                                                28


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                     Transitioning the Use of Cryptographic
      October 2024                                                                 Algorithms and Key Lengths

747       •    Digital signature verification:
748            When used for digital signature verification, SHA-1 is allowed for legacy use.
749       •    Applying cryptographic protection for non-digital signature applications:
750            The use of SHA-1 is deprecated through December 31, 2030, for applying protection in
751            non-digital signature applications and disallowed thereafter.
752       •    Processing already protected information using SHA-1 for non-digital signature
753            applications:
754            The use of SHA-1 is acceptable for processing already-protected information through
755            December 31, 2030, and allowed for legacy use thereafter.
756   Hash functions with a 224-bit output (i.e., SHA-224, SHA-512/224, and SHA3-224):
757       •    The use of 224-bit hash functions for applying cryptographic protection is deprecated
758            through December 31, 2030, and disallowed thereafter.
759       •    The use of 224-bit hash functions to process already protected information is acceptable
760            through December 31, 2030, and allowed for legacy use thereafter.
761   All other SHA-2 and SHA-3 hash functions (i.e., SHA-256, SHA-384, SHA-512, SHA-512/256, SHA3-
762   256, SHA3-384, and SHA3-512):
763            The use of these hash functions is acceptable for all hash function applications.
764   TupleHash and ParallelHash:
765            The use of TupleHash and ParallelHash is acceptable for the purposes specified in [SP 800-
766            185].
767




                                                       29


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                             Transitioning the Use of Cryptographic
      October 2024                                                                         Algorithms and Key Lengths

768   12. Extendable-Output Functions (XOFs)
769   Like hash functions, XOFs operate on input of an arbitrary length. The output of an XOF can be
770   extended to any desired length, whereas the output of a hash function is a predetermined fixed
771   length. Two XOFs are approved in [FIPS 202]: SHAKE128 and SHAKE256. [SP 800-185] provides
772   approved uses for these XOFs. Table 14 provides the approval status of the XOFs.
773                            Table 13. Approval status of eXtendable-Output Functions (XOFs)

                                 XOF                                                  Status
       SHAKE128
                                                                                    Acceptable
       SHAKE256

774   The use of SHAKE128 and SHAKE256 is acceptable when used as specified in approved
775   cryptographic applications.
776




                                                             30


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                                                Transitioning the Use of Cryptographic
      October 2024                                                                                            Algorithms and Key Lengths

777   13. Message Authentication Codes (MACs)
778   A MAC is a cryptographic checksum on the data over which it is computed and is used to provide
779   assurance of data integrity and source authentication. It is generated using a MAC algorithm and
780   a cryptographic key. The MAC can provide assurance that the data has not been modified since
781   the MAC was generated and that the MAC was computed by one of the parties sharing the key.
782   The key shall be generated and/or established using an approved method (e.g., using an
783   approved RBG or key-establishment scheme).
784   Three types of message authentication code mechanisms are specified for use:
785        1. [SP 800-224] specifies a keyed-hash message authentication code (HMAC) that uses a
786           hash function.
787        2. [SP 800-38B] and [SP 800-38D] 9 specify the CMAC and GMAC modes, respectively, for
788           block ciphers (i.e., AES and TDEA).
789        3. [SP 800-185] defines the KMAC algorithm that is based on the SHA-3 XOFs specified in
790           [FIPS 202].
791   The security strength that can be supported by a given MAC algorithm depends on the primitive
792   algorithm used (e.g., the hash function or block cipher) and the strength of the cryptographic
793   key. 10
794   Table 15 provides the approval status and required key strengths for the MAC algorithms and the
795   associated cryptographic primitives, as appropriate.
796                                                 Table 14. Approval status of MAC algorithms

                                                Cryptographic            Key Strengths (in
             MAC Algorithm                                                                                          Status
                                                  Primitive                    bits)
                                             SHA-1, SHA-2, SHA-3               < 112                         Disallowed
                                            SHA-1, SHA-224, SHA-                                      Deprecated through 2030
                                                                                ≥ 112
                                              512/224, SHA3-224                                        Disallowed after 2030
           HMAC Generation
                                                                         112 ≤ strength <             Acceptable through 2030
                                            All other SHA-2 and                                        Disallowed after 2030
                                                                               128
                                            SHA3 hash functions
                                                                              ≥ 128                          Acceptable

                                            SHA-1, SHA-2, SHA-3                 < 112                           Legacy use

                                            SHA-1, SHA-224, SHA-                                       Acceptable through 2030
                                                                                ≥ 112
                                             512/224, SHA3-224                                          Legacy use after 2030
           HMAC Verification
                                                                         112 ≤ strength <              Acceptable through 2030
                                            All other SHA-2 and                128                      Legacy use after 2030
                                            SHA3 hash functions
                                                                             ≥ 128 bits                        Acceptable

           CMAC Generation                       Two-key TDEA                   < 112                          Disallowed

      9 The CCM authenticated encryption mode specified in [SP 800-38C] also generates a MAC. However, the CCM mode cannot be used to generate

      a MAC without also performing encryption. The modes listed in this section are used only to generate a MAC.
      10 The strength of the key is less than or equal to its length.




                                                                          31


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                           Transitioning the Use of Cryptographic
      October 2024                                                                       Algorithms and Key Lengths

                                       Cryptographic      Key Strengths (in
           MAC Algorithm                                                                  Status
                                         Primitive              bits)
                                      Three-key TDEA             112                   Disallowed
                                            AES            128, 192, 256               Acceptable

                                           TDEA                 ≤ 112                  Legacy use
          CMAC Verification
                                  AES-128, AES-192 AES-
                                                           128, 192, 256               Acceptable
                                           256,
        GMAC Generation and
                                  AES-128, AES-192, 256    128, 192, 256               Acceptable
           Verification
                                                               < 112                   Disallowed
                                                          112 ≤ strength <      Acceptable through 2030
          KMAC Generation                  KMAC
                                                                128              Disallowed after 2030
                                                               ≥ 128                   Acceptable
                                                               < 112                   Disallowed
                                                          112 ≤ strength <      Acceptable through 2030
          KMAC Verification                KMAC
                                                                128              Legacy use after 2030
                                                               ≥ 128                   Acceptable

797   HMAC:
798       •    HMAC generation:
799                o HMAC generation using keys providing less than 112 bits of security strength is
800                  disallowed, regardless of the hash function used as the cryptographic primitive.
801                o HMAC generation using SHA-1 or the 224-bit hash functions (i.e., SHA-224, SHA-
802                  512/224, or SHA3-224) and keys providing ≥ 112 bits of security strength is
803                  deprecated through December 31, 2030, and disallowed thereafter.
804                o HMAC generation using all other hash functions (i.e., SHA-256, SHA-384, SHA-512,
805                  SHA-512/256, SHA3-256, SHA3-384, or SHA3-512):
806                             When using keys providing at least 112 bits of security strength but less
807                              than 128 bits of security strength, the use of these hash functions is
808                              acceptable through December 31, 2030, for HMAC generation and
809                              disallowed thereafter.
810                             When using keys providing at least 128 bits of security strength, the use of
811                              these hash functions is acceptable for HMAC generation.
812       •    HMAC Verification:
813                o HMAC verification using keys with less than 112 bits of security strength is allowed
814                  for legacy use.
815                o HMAC verification using SHA-1 or the 224-bit hash functions (i.e., SHA-224, SHA-
816                  512/224, or SHA3-224) and keys providing ≥ 112 bits of security strength is
817                  acceptable through December 31, 2030, and allowed for legacy use thereafter.


                                                           32


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                                        Transitioning the Use of Cryptographic
      October 2024                                                                                    Algorithms and Key Lengths

818                   o HMAC verification using all other hash functions (i.e., SHA-256, SHA-384, SHA-512,
819                     SHA-512/256, SHA3-256, SHA3-384, or SHA3-512):
820                                   When using keys providing at least 112 bits of security strength but less
821                                    than 128 bits of security strength, the use of these hash functions is
822                                    acceptable through December 31, 2030, for HMAC verification and
823                                    allowed for legacy use thereafter.
824                                   When using keys providing at least 128 bits of security strength, the use of
825                                    these hash functions is acceptable for HMAC verification.
826   CMAC:
827        •     CMAC Generation:
828                   o The use of TDEA for CMAC generation is disallowed.
829                   o The use of AES-128, AES-192, or AES-256 for CMAC generation is acceptable.
830        •     CMAC Verification:
831                   o The use of TDEA for CMAC verification is allowed for legacy use.
832                   o The use of AES for CMAC verification is acceptable.
833   GMAC Generation and Verification:
834        •     The use of GMAC for MAC generation is acceptable when using AES.
835   KMAC:
836        •     KMAC generation:
837                   o The use of KMAC for MAC generation using keys with less than 112 bits of security
838                     strength is disallowed.
839                   o The use of KMAC for MAC generation using keys with security strengths of at least
840                     112 bits but less than 128 bits is acceptable through December 31, 2030, and
841                     disallowed thereafter.
842                   o The use of KMAC for MAC generation using keys with at least 128 bits of security
843                     strength is acceptable.
844        •     KMAC verification:
845                   o The use of KMAC for MAC verification using keys with less than 112 bits of security
846                     strength is disallowed. 11
847                   o The use of KMAC for MAC verification using keys with security strengths of at least
848                     112 bits but less than 128 bits is acceptable through December 31, 2030, and
849                     allowed for legacy use thereafter.


      11 KMAC was initially approved after a security strength of 112 bits was required.




                                                                             33


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                    Transitioning the Use of Cryptographic
      October 2024                                                                Algorithms and Key Lengths

850                o The use of KMAC for MAC verification using keys with security strengths of at least
851                  128 bits is acceptable.




                                                      34


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                    Transitioning the Use of Cryptographic
      October 2024                                                                Algorithms and Key Lengths

852   References
853   [FIPS140]           National Institute of Standards and Technology (2019) Security Requirements
854                       for Cryptographic Modules. (Department of Commerce, Washington, DC),
855                       Federal Information Processing Standards Publication (FIPS) NIST FIPS 140-3.
856                       https://doi.org/10.6028/NIST.FIPS.140-3
857   [FIPS140_IG]        National Institute of Standards and Technology, Canadian Centre for Cyber
858                       Security Implementation Guidance for FIPS 140-3 and the Cryptographic Module
859                       Validation Program. Available at
860                       https://csrc.nist.gov/CSRC/media/Projects/cryptographic-module-validation-
861                       program/documents/fips%20140-3/FIPS%20140-3%20IG.pdf
862   [FIPS180]           National Institute of Standards and Technology (2015) Secure Hash Standard
863                       (SHS). (Department of Commerce, Washington, DC), Federal Information
864                       Processing Standards Publication (FIPS) NIST FIPS 180-4.
865                       https://doi.org/10.6028/NIST.FIPS.180-4
866   [FIPS186-4]         National Institute of Standards and Technology (2013) Digital Signature
867                       Standard (DSS). (Department of Commerce, Washington, DC), Federal
868                       Information Processing Standards Publication (FIPS) NIST FIPS 186-4
869                       [withdrawn].
870                      https://doi.org/10.6028/NIST.FIPS.186-4
871   [FIPS186-5]         National Institute of Standards and Technology (2023) Digital Signature
872                       Standard (DSS). (Department of Commerce, Washington, DC), Federal
873                       Information Processing Standards Publication (FIPS) NIST FIPS 186-5.
874                       https://doi.org/10.6028/NIST.FIPS.186-5
875   [FIPS197]           National Institute of Standards and Technology (2001) Advanced Encryption
876                       Standard (AES). (U.S. Department of Commerce, Washington, DC), Federal
877                       Information Processing Standards Publication (FIPS) NIST FIPS 197-upd1,
878                       updated May 9, 2023. https://doi.org/10.6028/NIST.FIPS.197-upd1
879   [FIPS202]           National Institute of Standards and Technology (2015) SHA-3 Standard:
880                       Permutation-Based Hash and Extendable-Output Functions. (U.S. Department of
881                       Commerce, Washington, DC), Federal Information Processing Standards
882                       Publication (FIPS) NIST FIPS 202. https://doi.org/10.6028/NIST.FIPS.202
883   [FIPS203]           National Institute of Standards and Technology (2024) Module-Lattice-Based
884                       Key-Encapsulation Mechanism Standard. (Department of Commerce,
885                       Washington, DC), Federal Information Processing Standards Publication (FIPS)
886                       NIST FIPS 203. https://doi.org/10.6028/NIST.FIPS.203
887   [FIPS204]           National Institute of Standards and Technology (2024) Module-Lattice-Based
888                       Digital Signature Standard. (Department of Commerce, Washington, DC),
889                       Federal Information Processing Standards Publication (FIPS) NIST FIPS 204.
890                       https://doi.org/10.6028/NIST.FIPS.204
891   [FIPS205]           National Institute of Standards and Technology (2024) Stateless Hash-Based
892                       Digital Signature Standard. (Department of Commerce, Washington, DC),



                                                      35


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                  Transitioning the Use of Cryptographic
      October 2024                                                              Algorithms and Key Lengths

893                 Federal Information Processing Standards Publication (FIPS) NIST FIPS 205.
894                 https://doi.org/10.6028/NIST.FIPS.205
895   [Grover]      Lov K. Grover. A fast quantum mechanical algorithm for database search. In
896                 Proceedings of the Twenty-Eighth Annual ACM Symposium on Theory of
897                 Computing, STOC ’96, page 212–219, New York, NY, USA, 1996. Association for
898                 Computing Machinery.
899   [RFC3526]     Kivinen T, Kojo M (2003) More Modular Exponential (MODP) Diffie-Hellman
900                 Groups for Internet Key Exchange (IKE), Internet Engineering Task Force (IETF)
901                 Request for Comments (RFC) 3526. https://doi.org/10.17487/RFC3526
902   [RFC4492]     Blake-Wilson S, Bolyard N (2006) Elliptic Curve Cryptography (ECC) Cipher Suites
903                 for Transport Layer Security (TLS), Internet Engineering Task Force (IETF)
904                 Request for Comments (RFC) 4492. https://doi.org/10.17487/RFC4492
905   [RFC5639]     Lochter M (2010) Elliptic Curve Cryptography (ECC) Brainpool Standard Curves
906                 and Curve Generation, Internet Engineering Task Force (IETF) Request for
907                 Comments (RFC)5639. https://doi.org/10.17487/RFC5639
908   [RFC5903]     Fu D, Salinas J (2010) Elliptic Curve Groups modulo a Prime (ECP Groups) for IKE
909                 and IKEv2, Internet Engineering Task Force (IETF) Request for Comments (RFC)
910                 5903. https://doi.org/10.17487/RFC5903
911   [RFC7919]     Gillmor D (2016) Negotiated Finite Field Diffie-Hellman Ephemeral Parameters
912                 for Transport Layer Security (TLS), Internet Engineering Task Force (IETF)
913                 Request for Comments (RFC) 7919. https://doi.org/10.17487/RFC7919
914   [RFC8017]     Moriarty K (ed.), Kaliski B, Jonsson J, Rusch A (2016) PKCS #1: RSA Cryptography
915                 Specifications Version 2.2. (Internet Engineering Task Force (IETF)), IETF Request
916                 for Comments (RFC) 8017. https://doi.org/10.17487/RFC8017
917   [SP80038A]    Dworkin MJ (2001) Recommendation for Block Cipher Modes of Operation:
918                 Methods and Techniques. (National Institute of Standards and Technology,
919                 Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-38A.
920                 https://doi.org/10.6028/NIST.SP.800-38A
921   [SP80038A_addendum]
922                 Dworkin MJ (2010) Recommendation for Block Cipher Modes of Operation:
923                 Three Variants of Ciphertext Stealing for CBC Mode. (National Institute of
924                 Standards and Technology, Gaithersburg, MD), NIST Special Publication (SP)
925                 NIST SP 800-38A, Addendum. https://doi.org/10.6028/NIST.SP.800-38A-Add
926   [SP80038B]    Dworkin MJ (2005) Recommendation for Block Cipher Modes of Operation: the
927                 CMAC Mode for Authentication. (National Institute of Standards and
928                 Technology, Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-38B,
929                 Includes updates as of October 6, 2016. https://doi.org/10.6028/NIST.SP.800-
930                 38B
931   [SP80038C]    Dworkin MJ (2004) Recommendation for Block Cipher Modes of Operation: the
932                 CCM Mode for Authentication and Confidentiality. (National Institute of
933                 Standards and Technology, Gaithersburg, MD), NIST Special Publication (SP)
934                 NIST SP 800-38C, Includes updates as of July 20, 2007.
935                 https://doi.org/10.6028/NIST.SP.800-38C

                                                      36


---

      NIST SP 800-131Ar3 ipd (Initial Public Draft)                     Transitioning the Use of Cryptographic
      October 2024                                                                 Algorithms and Key Lengths

936   [SP80038D]          Dworkin MJ (2007) Recommendation for Block Cipher Modes of Operation:
937                       Galois/Counter Mode (GCM) and GMAC. (National Institute of Standards and
938                       Technology, Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-38D.
939                       https://doi.org/10.6028/NIST.SP.800-38D
940   [SP80038E]          Dworkin MJ (2010) Recommendation for Block Cipher Modes of Operation: The
941                       XTS-AES Mode for Confidentiality on Storage Devices. (National Institute of
942                       Standards and Technology, Gaithersburg, MD), NIST Special Publication (SP)
943                       NIST SP 800-38E. https://doi.org/10.6028/NIST.SP.800-38E
944   [SP80038F]          Dworkin MJ (2012) Recommendation for Block Cipher Modes of Operation:
945                       Methods for Key Wrapping. (National Institute of Standards and Technology,
946                       Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-38F.
947                       https://doi.org/10.6028/NIST.SP.800-38F
948   [SP80038G]          Dworkin MJ (2016) Recommendation for Block Cipher Modes of Operation:
949                       Methods for Format-Preserving Encryption. (National Institute of Standards and
950                       Technology, Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-38G.
951                       https://doi.org/10.6028/NIST.SP.800-38G
952   [SP80056Ar2]        Barker EB, Chen L, Roginsky A, Smid M (2013) Recommendation for Pair-Wise
953                       Key-Establishment Schemes Using Discrete Logarithm Cryptography. (National
954                       Institute of Standards and Technology, Gaithersburg, MD), NIST Special
955                       Publication        (SP)      NIST        SP       800-56Ar2       [withdrawn].
956                       https://doi.org/10.6028/NIST.SP.800-56Ar2
957   [SP80056Ar3]        Barker EB, Chen L, Roginsky A, Vassilev A, Davis R (2018) Recommendation for
958                       Pair-Wise Key-Establishment Using Schemes Using Discrete Logarithm
959                       Cryptography. (National Institute of Standards and Technology, Gaithersburg,
960                       MD),      NIST     Special    Publication    (SP)    NIST    SP     800-56Ar3.
961                       https://doi.org/10.6028/NIST.SP.800-56Ar3
962   [SP80056B]          Barker E, Chen L, Regenscheid A, Smid M (2009) Recommendation for Pair-Wise
963                       Key-Establishment Schemes Integer Factorization Cryptography. (National
964                       Institute of Standards and Technology, Gaithersburg, MD), NIST Special
965                       Publication         (SP)      NIST        SP       800-56B        [withdrawn].
966                       https://doi.org/10.6028/NIST.SP.800-56B
967   [SP80056Br2]        Barker EB, Chen L, Roginsky A, Vassilev A, Davis R, Simon S (2019)
968                       Recommendation for Pair-Wise Key-Establishment Schemes Integer
969                       Factorization Cryptography. (National Institute of Standards and Technology,
970                       Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-56Br2.
971                       https://doi.org/10.6028/NIST.SP.800-56Br2
972   [SP80056C]          Barker EB, Chen L, Davis R (2020) Recommendation for Key-Derivation Methods
973                       in Key-Establishment Schemes. (National Institute of Standards and Technology,
974                       Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-56Cr2.
975                       https://doi.org/10.6028/NIST.SP.800-56Cr2
976   [SP80057]           Barker EB (2020) Recommendation for Key Management: Part 1 – General.
977                       (National Institute of Standards and Technology, Gaithersburg, MD), NIST


                                                       37


---

       NIST SP 800-131Ar3 ipd (Initial Public Draft)                 Transitioning the Use of Cryptographic
       October 2024                                                             Algorithms and Key Lengths

 978                 Special        Publication         (SP)       NIST        SP       800-57pt1r5.
 979                 https://doi.org/10.6028/NIST.SP.800-57pt1r5
 980   [SP80067]     Barker EB, Mouha N (2017) Recommendation for the Triple Data Encryption
 981                 Algorithm (TDEA) Block Cipher. (National Institute of Standards and Technology,
 982                 Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-67r2.
 983                 https://doi.org/10.6028/NIST.SP.800-67r2
 984   [SP80090A]    Barker EB, Kelsey JM (2015) Recommendation for Random Number Generation
 985                 Using Deterministic Random Bit Generators. (National Institute of Standards and
 986                 Technology, Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-90Ar1.
 987                 https://doi.org/10.6028/NIST.SP.800-90Ar1
 988   [SP80090B]    Turan MS, Barker EB, Kelsey JM, McKay KA, Baish ML, Boyle VM (2018)
 989                 Recommendation for the Entropy Sources Used for Random Bit Generation.
 990                 (National Institute of Standards and Technology, Gaithersburg, MD), NIST
 991                 Special Publication (SP) NIST SP 800-90B. https://doi.org/10.6028/NIST.SP.800-
 992                 90B
 993   [SP80090C]    Barker EB, Kelsey JM, McKay KA, Roginsky A, Turan MS (2024) Recommendation
 994                 for Random Bit Generator (RBG) Constructions. (National Institute of Standards
 995                 and Technology, Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-
 996                 90C 4pd. https://doi.org/10.6028/NIST.SP.800-90C.4pd
 997   [SP800108]    Chen L (2022) Recommendation for Key Derivation Using Pseudorandom
 998                 Functions (Revised). (National Institute of Standards and Technology,
 999                 Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-108r1-upd1,
1000                 Includes updates as of February 2, 2024. https://doi.org/10.6028/NIST.SP.800-
1001                 108r1-upd1
1002   [SP800131Ar2] Barker E, Roginsky A (2019) Transitions: Recommendation for Transitioning the
1003                 Use of Cryptographic Algorithms and Key Lengths. (National Institute of
1004                 Standards and Technology, Gaithersburg, MD), NIST Special Publication (SP)
1005                 NIST SP 800-131Ar2. http://dx.doi.org/10.6028/NIST.SP.800-131Ar2
1006   [SP800132]    Sönmez Turan M, Barker EB, Burr WE, Chen L (2010) Recommendation for
1007                 Password-Based Key Derivation: Part 1: Storage Applications. (National Institute
1008                 of Standards and Technology, Gaithersburg, MD), NIST Special Publication (SP)
1009                 NIST SP 800-132. https://doi.org/10.6028/NIST.SP.800-132
1010   [SP800133]    Barker E, Roginsky A, Davis R (2020) Recommendation for Key Generation.
1011                 (National Institute of Standards and Technology, Gaithersburg, MD), NIST
1012                 Special        Publication          (SP)       NIST        SP        800-133r2.
1013                 https://doi.org/10.6028/NIST.SP.800-133r2
1014   [SP800135]    Dang Q (2011) Recommendation for Existing Application-Specific Key Derivation
1015                 Functions. (National Institute of Standards and Technology, Gaithersburg, MD),
1016                 NIST       Special      Publication      (SP)      NIST      SP      800-135r1.
1017                 https://doi.org/10.6028/NIST.SP.800-135r1
1018   [SP800185]    Kelsey JM, Chang S-jH, Perlner RA (2016) SHA-3 Derived Functions: cSHAKE,
1019                 KMAC, TupleHash, and ParallelHash. (National Institute of Standards and


                                                       38


---

       NIST SP 800-131Ar3 ipd (Initial Public Draft)                       Transitioning the Use of Cryptographic
       October 2024                                                                   Algorithms and Key Lengths

1020                       Technology, Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-185.
1021                       https://doi.org/10.6028/NIST.SP.800-185
1022   [SP800186]          Chen L, Moody D, Regenscheid A, Robinson A, Randall K (2023)
1023                       Recommendations for Discrete Logarithm-based Cryptography: Elliptic Curve
1024                       Domain Parameters (National Institute of Standards and Technology,
1025                       Gaithersburg, MD), NIST Special Publication (SP) NIST SP 800-186.
1026                       https://doi.org/10.6028/NIST.SP.800-186
1027   [SP800208]          Cooper DA, Apon DC, Dang QH, Davidson MS, Dworkin MJ, Miller CA (2020)
1028                       Recommendation for Stateful Hash-Based Signature Schemes, (National
1029                       Institute of Standards and Technology, Gaithersburg, MD), NIST Special
1030                       Publication (SP) NIST SP 800-208. https://doi.org/10.6028/NIST.SP.800-208
1031   [SP800224]          Sönmez Turan M, Brandão LTAN (2024) Keyed-Hash Message Authentication
1032                       Code (HMAC): Specification of HMAC and Recommendations for Message
1033                       Authentication (National Institute of Standards and Technology, Gaithersburg,
1034                       MD), NIST Special Publication (SP) NIST SP 800-224 ipd.
1035                       https://doi.org/10.6028/NIST.SP.800-224.ipd
1036   [NISTIR8413]        Alagic G, Apon D, Cooper D, Dang Q, Dang T, Kelsey J, Lichtinger J, Liu Y, Miller
1037                       C, Moody D, Peralta R, Perlner R, Robinson A, Smith-Tone D (2022) Status Report
1038                       on the Third Round of the NIST Post-Quantum Cryptography Standardization
1039                       Process. (National Institute of Standards and Technology, Gaithersburg, MD),
1040                       NIST Interagency or Internal Report (IR) NIST IR 8413-upd1, Includes updates as
1041                       of September 26, 2022. https://doi.org/10.6028/NIST.IR.8413-upd1
1042   [NISTIR8459]        Mouha N, Dworkin M (2024) Report on the Block Cipher Modes of Operation in
1043                       the NIST SP 800-38 Series. (National Institute of Standards and Technology,
1044                       Gaithersburg, MD), NIST Interagency or Internal Report (IR) NIST IR 8459 ipd.
1045                       https://doi.org/10.6028/NIST.IR.8459
1046   [X931]              American National Standard (ANS) X9.31-1998, Digital Signatures Using
1047                       Reversible Public Key Cryptography for the Financial Services Industry (rDSA).
1048                       Withdrawn.
1049   [Zalka]             C. Zalka, Grover’s quantum searching algorithm is optimal, Phys. Rev. A 60, 2746
1050                       (1999).
1051




                                                         39


---

       NIST SP 800-131Ar3 ipd (Initial Public Draft)                     Transitioning the Use of Cryptographic
       October 2024                                                                 Algorithms and Key Lengths

1052   Appendix A. Continued Use of AES
1053   Grover’s algorithm [Grover] allows a quantum computer to perform a brute-force key search
1054   using (approximately) the square root of the number of steps that would be required using a
1055   classical computer. This suggests that an attacker with access to a quantum computer might be
1056   able to attack a symmetric cipher with a key up to twice as long as could be attacked by an
1057   attacker with access to only classical computers. However, quantum computing hardware will
1058   likely be more expensive to build and use than classical hardware, and Grover’s algorithm might
1059   not speed up brute-force key search as dramatically as suspected.
1060   In 1997, Zalka proved that in order to obtain the full quadratic speedup, all of the steps of
1061   Grover’s algorithm must be performed in series [Zalka]. The advantage of Grover’s algorithm will
1062   be smaller in the real world, where attacks on cryptography use massively parallel processing.
1063   Taking this into account, it is quite likely that Grover’s algorithm will provide less than the
1064   expected advantage in attacking AES. Furthermore, even if quantum computers become much
1065   less expensive than anticipated, the known difficulty of parallelizing Grover’s algorithm suggests
1066   that AES will still be safe for a very long time. This, of course, assumes that no new cryptographic
1067   weaknesses with respect to classical or quantum cryptanalysis are found in AES.
1068   Based on this understanding, current applications can continue to use AES with key sizes of 128,
1069   192, or 256 bits. When NIST foresees the need for a transition of symmetric key algorithms, hash
1070   functions, key-establishment methods, or digital signature schemes to protect against threats
1071   from quantum computers, NIST and the CMVP will issue guidance regarding such transitions.
1072




                                                       40


---

       NIST SP 800-131Ar3 ipd (Initial Public Draft)                    Transitioning the Use of Cryptographic
       October 2024                                                                Algorithms and Key Lengths

1073   Appendix B. Change History
1074   Revision 3 includes the following changes to SP 800-131A:
1075       1. The document has been reformatted using a revised template for NIST Special
1076          Publications (SPs).
1077       2. Section 1.2.2 expands the discussion on legacy use to include how users should consider
1078          the legacy-use status.
1079       3. Section 1.2.3 is a new section that discusses the strategy for transitioning from a 112-bit
1080          security strength to a 128-bit security strength for block ciphers and hash functions or
1081          continuing the acceptability of the 112-bit security strength until further PQC guidance is
1082          provided for digital signatures and key establishment.
1083       4. In Sec. 2, the Skipjack algorithm has been removed, TDEA is disallowed for applying
1084          cryptographic protection, and a subsection on the block cipher modes of operation has
1085          been added.
1086       5. In Sec. 3, EdDSA (specified in [FIPS 186]), the new quantum-resistant digital signature
1087          algorithms (specified in [FIPS 204] and [FIPS 205]), and the stateful hash-based signature
1088          algorithms (specified in [SP 800-208]) have been added. DSA and RSA (as specified in
1089          [X9.31]) are now disallowed for generating digital signatures.
1090       6. Sections 4, 8, and 12 have been added to discuss key generation [SP 800-133], key
1091          encapsulation mechanisms [FIPS 203], and XOFs [FIPS 202].
1092       7. Section 5 has been augmented to include entropy sources [SP 800-90B] and RBG
1093          constructions [SP 800-90C]. The discussion of Dual_EC_DRBG RNGs has been removed.
1094       8. Sections 6 and 7 have been updated to show that DH, MQV, and RSA schemes that do not
1095          comply with [SP 800-56A] or [SP 800-56B] are only allowed for legacy use.
1096       9. Section 9 has added the one- and two-step key-derivation methods specified in [SP 800-
1097          56C] and key derivation [SP 800-108]. HMAC using SHA-1 and the 224-bit hash functions
1098          has been deprecated.
1099       10. In Sec. 10, key wrapping using TDEA is disallowed, and CCM and GCM have been added.
1100           The combination of an approved encryption mode and an approved authentication
1101           method for key wrapping has been deprecated.
1102       11. In Sec. 11, the use of SHA-1 and the 224-bit hash functions has been deprecated.
1103       12. In Sec. 13, the use of SHA-1 and the 224-bit hash functions for generating a MAC has been
1104           deprecated.
1105       13. The References section has been updated.
1106       14. Appendix A has been added to discuss the continued use of AES when quantum
1107           computers become available.
1108       15. Appendix C includes a list of the acronyms used in this document.

                                                       41


---

       NIST SP 800-131Ar3 ipd (Initial Public Draft)        Transitioning the Use of Cryptographic
       October 2024                                                    Algorithms and Key Lengths

1109       16. Appendix D provides a glossary of terms.




                                                       42


---

       NIST SP 800-131Ar3 ipd (Initial Public Draft)              Transitioning the Use of Cryptographic
       October 2024                                                          Algorithms and Key Lengths

1110   Appendix C. List of Symbols, Abbreviations, and Acronyms
1111   AES
1112   Advanced Encryption Standard

1113   CAVP
1114   Cryptographic Algorithm Validation Program

1115   CMVP
1116   Cryptographic Module Validation Program

1117   DRBG
1118   Deterministic Random Bit Generator

1119   FIPS
1120   Federal Information Processing Standards

1121   ITL
1122   Information Technology Laboratory

1123   MAC
1124   Message Authentication Code

1125   MQV
1126   Menezes-Qu-Vanstone (algorithm)

1127   NIST
1128   National Institute of Standards and Technology

1129   RBG
1130   Random Bit Generator

1131   SP
1132   (NIST) Special Publication

1133   TDEA
1134   Triple Data Encryption Algorithm
1135




                                                        43


---

       NIST SP 800-131Ar3 ipd (Initial Public Draft)                                 Transitioning the Use of Cryptographic
       October 2024                                                                             Algorithms and Key Lengths

1136   Appendix D. Glossary
1137   acceptable
1138   The algorithm and key length/strength in a FIPS or SP is approved for use in accordance with any associated
1139   guidance.

1140   apply cryptographic protection
1141   Depending on the algorithm, to encrypt or sign data, generate a hash function or message authentication code
1142   (MAC), or establish keys, including wrapping and deriving keys.

1143   approval status
1144   Used to designate usage by the U.S. Federal Government.

1145   approved
1146   FIPS-approved or NIST-recommended. An algorithm or technique that is either 1) specified in a FIPS or NIST
1147   recommendation or 2) adopted in a FIPS or NIST recommendation and specified in (a) an appendix to the FIPS or
1148   NIST recommendation or (b) a document referenced by a FIPS or NIST recommendation.

1149   deprecated
1150   The algorithm and key length may be used, but there is some security risk.

1151   disallowed
1152   The algorithm or key length is no longer allowed for applying cryptographic protection.

1153   entropy
1154   A measure of disorder, randomness, or variability in a closed system.

1155   hash(ing) method
1156   A hash function or extendable-output function.

1157   legacy use
1158   The algorithm or key length may only be used to process already protected information (e.g., decrypt ciphertext
1159   data or verify a digital signature).

1160   len(x)
1161   The length of x in bits.

1162   security strength
1163   A number associated with the amount of work (i.e., the number of operations) that is required to break a
1164   cryptographic algorithm or system. If 2N execution operations of the algorithm (or system) are required to break
1165   the cryptographic algorithm, then the security strength is N bits. As used herein, security strength is a measure of
1166   the difficulty of subverting cryptographic protection (e.g., discovering the key) using classical computers.

1167   shall
1168   A requirement for Federal Government use. Shall may be coupled with not to become shall not.

1169   XOR(ing)
1170   Bit-wise exclusive-or. A mathematical operation that is defined as 0 ⊕ 0 = 0, 0 ⊕ 1 = 1, 1 ⊕ 0 = 1, and 1 ⊕ 1 = 0.




                                                                44


---

