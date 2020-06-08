# pysam pileup()

##SAM文件对 pileup信息的解释
'''
Information on match, mismatch, indel, strand, mapping
quality and start and end of a read are all encoded at the
read base column. At this column, a dot stands for a match
to the reference base on the forward strand, a comma for a
match on the reverse strand, a '>' or '<' for a reference
skip, 'ACGTN' for a mismatch on the forward strand and
'acgtn' for a mismatch on the reverse strand. A pattern
'\+[0-9]+[ACGTNacgtn]+' indicates there is an insertion
between this reference position and the next reference
position. The length of the insertion is given by the
integer in the pattern, followed by the inserted
sequence. Similarly, a pattern '-[0-9]+[ACGTNacgtn]+'
represents a deletion from the reference. The deleted bases
will be presented as '*' in the following lines. Also at
the read base column, a symbol '^' marks the start of a
read. The ASCII of the character following '^' minus 33
gives the mapping quality. A symbol '$' marks the end of a
read segment
'''

## pysam pileup 的使用
'''python
import pysam  
samfile = pysam.AlignmentFile("ex1.bam", "rb" )  
for pileupcolumn in samfile.pileup("chr1", 100, 120):  
    print ("\ncoverage at base %s = %s" %  
           (pileupcolumn.pos, pileupcolumn.n))  
    for pileupread in pileupcolumn.pileups:  
        if not pileupread.is_del and not pileupread.is_refskip:  
            # query position is None if is_del or is_refskip is set.  
            print ('\tbase in read %s = %s' %
                  (pileupread.alignment.query_name,
                   pileupread.alignment.query_sequence[pileupread.query_position]))

samfile.close()
'''

'''
samfile.pileup("chr1", 100, 120)可以获取基因组一段位置的pileup情况
该region中的每个position(单个碱基)处都有一个pileupcolumn

'''