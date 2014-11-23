==============
Summarize and explore the functional annotation
==============
Now that we have annotated genes and quantified them in the sample using read mapping we will summarize and explore the annotations. This we will do by producing interactive plots detailing the proportion of functional categories such as metabolic pathways and orthologous gene families.

KRONA interactive plots
=========
KRONA is a tool that takes as input a table of abundance values and several hierarchical categories and produces HTML files that can be explored interactively. The enzyme annotations from PROKKA are particularly suited for this purpose because these annotations can be grouped into higher functional categories such as pathways (e.g. glycolysis) and pathway classes (e.g. energy metabolism) for enzymes. Similarly, COG annotations can be summed up into higher categories such as "Carbohydrate transport and metabolism" and "Metabolism".

First we will create a new directory for the krona output and link to the necessary files::

  mkdir -p ~/mg-workshop/results/$SAMPLE/functional_annotation/krona
  cd ~/mg-workshop/results/$SAMPLE/functional_annotation/krona
  ln -s ~/mg-workshop/results/$SAMPLE/functional_annotation/prokka/PROKKA.$SAMPLE.ec
  ln -s ~/mg-workshop/results/$SAMPLE/functional_annotation/prokka/PROKKA.$SAMPLE.cog
  ln -s ~/mg-workshop/results/$SAMPLE/functional_annotation/minpath/PROKKA.$SAMPLE.metacyc.minpath
  ln -s ~/mg-workshop/results/$SAMPLE/functional_annotation/minpath/PROKKA.$SAMPLE.kegg.minpath
  ln -s ~/mg-workshop/results/$SAMPLE/functional_annotation/prokka/PROKKA.$SAMPLE.genelengths
  ln -s ~/mg-workshop/results/$SAMPLE/functional_annotation/mapping/bowtie2/$SAMPLE.coverage

Next, use the genes.to.kronaTable.py script to produce the tabular output needed for KRONA.

For Metacyc pathways (from enzymes, only considering pathways predicted by MinPath)::

  genes.to.kronaTable.py -i PROKKA.$SAMPLE.ec -m ~/mg-workshop/data/db/metacyc/ec.to.pwy -H ~/mg-workshop/data/db/metacyc/pwy.hierarchy -n $SAMPLE -l <(grep "minpath 1" PROKKA.$SAMPLE.metacyc.minpath) -c $SAMPLE.coverage -L PROKKA.$SAMPLE.genelengths -o $SAMPLE.krona.metacyc.minpath.tab
  
For KEGG pathways (from enzymes, only considering pathways predicted by MinPath)::

  genes.to.kronaTable.py -i PROKKA.$SAMPLE.ec -m ~/mg-workshop/data/db/kegg/ec.to.pwy -H ~/mg-workshop/data/db/kegg/pwy.hierarchy -n $SAMPLE -l <(grep "minpath 1" PROKKA.$SAMPLE.kegg.minpath) -c $SAMPLE.coverage -L PROKKA.$SAMPLE.genelengths -o $SAMPLE.krona.kegg.minpath.tab

For COG annotations::

  genes.to.kronaTable.py -i PROKKA.$SAMPLE.cog -m ~/mg-workshop/data/db/cog/cog.to.cat -H ~/mg-workshop/data/db/cog/cat.hierarchy -n $SAMPLE -c $SAMPLE.coverage -L PROKKA.$SAMPLE.genelengths -o $SAMPLE.krona.COG.tab
  
Then use the ktImportText script to generate the HTML files::

  ktImportText -o $SAMPLE.krona.metacyc.minpath.html $SAMPLE.krona.metacyc.minpath.tab
  ktImportText -o $SAMPLE.krona.kegg.minpath.html $SAMPLE.krona.kegg.minpath.tab
  ktImportText -o $SAMPLE.krona.COG.html $SAMPLE.krona.COG.tab