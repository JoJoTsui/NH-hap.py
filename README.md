# Benchmarking

# reference

[https://github.com/ga4gh/benchmarking-tools/blob/master/resources/high-confidence-sets/platinumgenomes.md](https://github.com/ga4gh/benchmarking-tools/blob/master/resources/high-confidence-sets/platinumgenomes.md)

# working directory

`/storeData/project/user/xuzhenyu/workspace/germline/test_call/ERP001960`

# workflow

## Download truth set

```bash
~~wget ftp://platgene_ro@ussd-ftp.illumina.com/2016-1.0/hg38/small_variants/NA12878/NA12878.vcf.gz
wget ftp://platgene_ro@ussd-ftp.illumina.com/2016-1.0/hg38/small_variants/NA12878/NA12878.vcf.gz.tbi
wget ftp://platgene_ro@ussd-ftp.illumina.com/2016-1.0/hg38/small_variants/ConfidentRegions.bed.gz~~

# From the Illumina FTP site: ftp://ussd-ftp.illumina.com/2016-1.0/

# If this link doesn't work in your browser try to directly ftp to ussd-ftp.illumina.com, with username=platgene_ro and empty password.
```

## Download raw data

```bash
# ERP001960: http://www.ebi.ac.uk/ena/data/view/ERP001960
# ERR194147 = NA12878
# download using aspera
/home/xuzhenyu/.aspera/connect/bin/ascp -v -i /home/xuzhenyu/.aspera/connect/etc/asperaweb_id_dsa.openssh -k 1 -QT -l 300m -P33001 era-fasp@fasp.sra.ebi.ac.uk:vol1/fastq/ERR194/ERR194147/ERR194147_1.fastq.gz ./raw/ERR194147/
/home/xuzhenyu/.aspera/connect/bin/ascp -v -i /home/xuzhenyu/.aspera/connect/etc/asperaweb_id_dsa.openssh -k 1 -QT -l 300m -P33001 era-fasp@fasp.sra.ebi.ac.uk:vol1/fastq/ERR194/ERR194147/ERR194147_2.fastq.gz ./raw/ERR194147/
```

## Call germline variants using our own pipeline

```bash
bash /storeData/project/user/xuzhenyu/pipeline/germline/germline.wgs.sh \
    /storeData/project/user/xuzhenyu/workspace/germline/test_call/ERP001960/raw/ERR194147/sample.list \
    /storeData/project/user/xuzhenyu/database/genome/hg19/hg19.fa \
    /storeData/project/user/xuzhenyu/workspace/germline/test_call/ERP001960/ERR194147 \
    vcf \
    20
```

## benchmarking

```bash
hap.py \
    /storeData/project/user/xuzhenyu/workspace/germline/test_call/ERP001960/validation/NA12878.vcf.gz \
    /storeData/project/user/xuzhenyu/workspace/germline/test_call/ERP001960/ERR194147/4.germline/ERR194147/ERR194147.germline.vcf \
    -f /storeData/project/user/xuzhenyu/workspace/germline/test_call/ERP001960/validation/ConfidentRegions.bed.gz \
    -o ERR194147 \
    -r /storeData/project/user/xuzhenyu/database/genome/hg19/hg19.fa
```

# summary

[ERR194147 metric](https://www.notion.so/b02157b853b84593be9b86ab8f6d1164)

- `METRIC.Recal`
- `METRIC.Precision`
- `METRIC.F1_Score`