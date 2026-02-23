## Tutorial: Comparing HK97-fold protein structures with DALI (all-vs-all)

This walkthrough shows how to reproduce a simple *structure-based* comparison of HK97-fold proteins (encapsulins vs phage capsid proteins) using the **DALI** server’s **all-versus-all** mode, and how to interpret the output as an evolutionary “first pass”.

### Context

HK97-fold proteins show up in two big contexts:

* **Encapsulins**: cellular protein nanocompartments built from HK97-fold subunits.
* **Phage major capsid proteins (MCPs)**: viral HK97-fold subunits that assemble into phage heads.

A structural dendrogram is a strong way to see which proteins are most similar in 3D and whether “cellular vs viral” groups cluster or mix.

This workflow is inspired by the structural comparisons shown in **Fig. 7** of *Large-scale computational discovery and analysis of virus-derived microbial nanocompartments* (Michael P. Andreas; Tobias W. Giessen). ([1][1])

---

## 1) Gather the structures you will compare

You need a set of representative HK97-fold structures. In this case, the examples come from Fig. 7 of Andreas & Giessen (Nature Communications, 2021). ([1][1])

We compare **monomers only** (single subunits), not full capsids.

---

## 2) Build a labeled list of PDB IDs and chains

DALI accepts identifiers in the form:

* `PDBID + chain` (e.g., `4PT2A` means PDB `4PT2`, chain `A`)

A common convention for these assemblies is that the main capsid subunit monomer is available as **chain A**, so we use chain A consistently here.

Use the list below exactly as input (one per line). The label after each ID is for *your* tracking (DALI will mainly use the structure identifiers):

```text
4PT2A T3_Encapsulin
6NJ8A T4_Encapsulin
2E0ZA T3_Encapsulin
1OHGA HK97_Phage
5TJTA T5_Phage
6X8TA T1_cNMP_Encapsulin
3BJQA RB50_Phage
1YUEA T4_Phage
2PK8A A_domain_Encapsulin
3J4UA BPP1_Phage
```

### Labels source:

* **Encapsulin vs Phage**: the main biological category you care about.
* **T number (T=1,3,4,5, …)**: an icosahedral triangulation number; in practice it’s a rough proxy for capsid **size/complexity** in many HK97-fold assemblies (useful as a qualitative annotation).

---

## 3) Submit an “all-versus-all” job to DALI

1. Go to the DALI [server](http://ekhidna2.biocenter.helsinki.fi/dali/):
   `http://ekhidna2.biocenter.helsinki.fi/dali/`

2. Click **“all versus all”**.

3. On that page, click the **alternative submission form** [link](http://ekhidna2.biocenter.helsinki.fi/dali/submitmatrix.html) (“you can use this alternative submission form”), which takes you here:
   `http://ekhidna2.biocenter.helsinki.fi/dali/submitmatrix.html`

4. In **“STEP 1 - Enter your input protein structures”**, paste the block of identifiers.

5. Fill in:

   * A **job title** (example: `HK97_all_vs_all_monomers`)
   * Your **email** (DALI sends the result link)

6. Submit the job.

**Runtime:** often around ~1 hour, but it depends on server load and queue time.

---

## 4) What you get back:

DALI returns results typically including:

* A **structural dendrogram** (tree-like clustering of your structures)
* A **pairwise similarity matrix** (often a heatmap of structural similarity)
* Pairwise alignment statistics (commonly including **Z-scores**)

**Record these:**

* The **result URL**
* The **dendrogram image / downloadable tree**
* The **matrix/heatmap**
* Any downloadable tables (pairwise scores)

This makes your analysis reproducible even if the server later purges old jobs.

---

## 5) How to interpret the dendrogram:

1. **Look for grouping by category**

   * Do encapsulins cluster together?
   * Do phage MCPs cluster together?
   * Are there mixed branches?

2. **Check where “special” encapsulins land**

   * `A_domain_Encapsulin (2PK8A)` is a truncated HK97-like A-domain; you might expect it to sit apart from full-length capsid proteins, or near proteins where the A-domain dominates the structural signal. (This matches the intuition discussed around Fig. 7 in Andreas & Giessen.) ([1][1])

3. **Relate structure clusters to the “T number”**

   * Sometimes higher-complexity capsid proteins share insertions/extended arms that shift structural similarity.
   * Sometimes the fold core dominates, and T doesn’t strongly separate.

---

## 6) Compare your dendrogram to a broader capsid-evolution framework

Once you understand the DALI dendrogram, you can compare the qualitative message to the conclusions and dendrogram of:

* **Multiple origins of viral capsid proteins from cellular ancestors** (Mart Krupovic; Eugene V. Koonin). ([2][2])

---

[1]: https://www.nature.com/articles/s41467-021-25071-y "Large-scale computational discovery and analysis of virus-derived microbial nanocompartments | Nature Communications"
[2]: https://pubmed.ncbi.nlm.nih.gov/28265094/?utm_source=chatgpt.com "Multiple origins of viral capsid proteins from cellular ancestors - PubMed"
