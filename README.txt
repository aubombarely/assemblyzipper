INTRODUCTION:

     AssemblyZipper will be a pipeline to integrate two assemblies from
  the same species.

     One of the assemblies should be defined as Reference (usually the bigger),
  and the other as Query. This pipeline will have the following steps:

  1. Query fragmentation:
     
         Query assembly will be fragmented into 100 bp fragments.

  2. Query framents mapping:
  
        These fragments will be mapped with the Reference asssembly using a NGS
     mapping tool such as bwa.

  3. Overlaps processing and classification:

        The mapping coordinates will be processed and the Query sequences will
     be classified according the match and the overlap.

     +-------------+----------+------------------------+--------------------+
     | Match Count | Extreme  | Overlap Type           | Classification     |
     +-------------+----------+------------------------+--------------------+
     | 1           | NA       | Full                   | Redundant     (R)  |
     | 1           | 5        | Partial                | Extension     (E5) |
     | 1           | 3        | Partial                | Extension     (E3) |
     | 2           | 5,3      | Partial No Overlapping | Gap-Join      (JG) |
     | 2           | 5,5      | Partial Overlapping    | Bifu.Conflict (CB5)|
     | 2           | 3,3      | Partial Overlapping    | Bifu.Conflict (CB3)|
     | 2           | 5,3      | Partial Overlapping    | Overlap-Join  (JO) |
     | >2          | Multiple | Partial                | Mult.Conflict (CM) |
     +-------------+----------+------------------------+--------------------+

        In the same way Reference sequences will be classified according the
     number of matches.

  4. Sequence Clustering:

        It will define Zip-Starting-Points (ZPT). These points will be Query
     sequences classified as 5' Extension (E5) or 3' Extension (E3) followed by
     Reference sequences classified as 3' Extension (E3), 5' Extension (E5) or 
     Joins (JO or JG). Once the ZPT will be defined, it will look into the 
     extension until it finds a dead end (E3) or (E5), or a conflict (CB3, CM).

  5. Sequence Integration and consensus sequence.

        CAP3 will be used to assembly the sequences from the same cluster.
     Ace files will be processed to .gff3 containg information about the 
     integration.
