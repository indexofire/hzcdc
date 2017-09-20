# Biopython

## 模块列表

* Bio.Seq
* Bio.SeqRecord
* Bio.SeqIO
* Bio.SeqFeature
* Bio.Align
* Bio.AlignIO
* Bio.Application


### Application

* Bio.Align.Applications
* Bio.Blast.Applications
* Bio.Emboss.Applications
* Bio.Sequencing.Applications

#### Emboss

- Diffseq
- EInverted
- ETandem
- Est2Genome
- FConsense
- FDNADist
- FDNAPars
- FNeighbor
- FProtDist
- FProtPars
- FSeqBoot
- FTreeDist
- Fuzznuc
- Iep
- Needle
- Needleall
- Palindrome
- Primer3
- PrimerSearch
- Seqmatchall
- Stretcher
- Tranalign
- Water

```python
from Bio.Emboss.Applications import WaterCommandline

cmd = WaterCommandline(gapopen=10, gapextend=0.5, stdout=True, auto=True,
    asequence="seq1.fasta", bsequence="seq2.fasta")
print("About to run: %s" % cmd)
std_output, err_output = cmd()
```
