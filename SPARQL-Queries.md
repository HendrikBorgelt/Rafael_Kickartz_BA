SPARQL Queries:

Prefix:

```
            PREFIX : <http://www.semanticweb.org/ontologies/reac4cat_exp_data_enhancement#>
            PREFIX dc: <http://purl.org/dc/elements/1.1/>
            PREFIX obo: <http://purl.obolibrary.org/obo/>
            PREFIX owl: <http://www.w3.org/2002/07/owl#>
            PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
            PREFIX xml: <http://www.w3.org/XML/1998/namespace>
            PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
            PREFIX foaf: <http://xmlns.com/foaf/0.1/>
            PREFIX om-2: <http://www.ontology-of-units-of-measure.org/resource/om-2/>
            PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
            PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
            PREFIX chebi: <http://purl.obolibrary.org/obo/chebi/>
            PREFIX terms: <http://purl.org/dc/terms/>
            PREFIX protege: <http://protege.stanford.edu/plugins/owl/protege#>
            PREFIX oboInOwl: <http://www.geneontology.org/formats/oboInOwl#>
            PREFIX property: <http://purl.allotrope.org/ontologies/property#>
            PREFIX ontologies: <https://nfdi4cat.org/ontologies/>
            PREFIX reac4cat_exp_data_link: <http://www.semanticweb.org/ontologies/reac4cat_exp_data_link#>
            PREFIX reac4cat_exp_data_enhancement: <http://www.semanticweb.org/ontologies/reac4cat_exp_data_enhancement#>
```

assignment desired_product_role:

```            
	    INSERT { ?NP :hasComponentRole :by_product_role .}
            WHERE { ?NP ontologies:reac4cat_000065 ?reaction .
    	            FILTER NOT EXISTS {?NP :hasComponentRole :desired_product_role}}
```

assignment educt_component_role:

```
                INSERT { ?INERTSOLVENT :hasComponentRole :inert_or_solvent_component_role .}
                WHERE { ?INERTSOLVENT ontologies:reac4cat_000009 ?reaction .
        	            FILTER NOT EXISTS {?INERTSOLVENT :hasComponentRole :educt_component_role}}
```

assignment heterogeneous_catalyst_role:

```
                INSERT { ?HetKat :hasCatalystRole :heterogeneous_catalyst_role .}
                WHERE { ?FixedBedReac rdf:type :FixedBedReactor .
                        ?FixedBedReac :isPartOf ?reaction .
                        ?HetKat :isCatalystSampleOf ?reaction . }
```

explicit reaction mapping:
```
catalysed FTS:
        INSERT { ?Reaction :hasFirstReaction :catalysed_fischer_tropsch_process_ind .}
            WHERE { ?Experiment rdf:type :CatalysisPerfomanceAssay .
                    ?Experiment :hasReaction ?Reaction .
                    ?alkane rdfs:label "alkane".
                    ?SubClassAlkane rdfs:subClassOf ?alkane .
                    ?alkane_ind rdf:type ?SubClassAlkane .
                    ?Dihydrogen rdf:type obo:CHEBI_18276 .
                    ?Reaction ontologies:reac4cat_000005 :Substance_CO .
                    ?Reaction ontologies:reac4cat_000005 ?Dihydrogen .
                    ?Reaction ontologies:reac4cat_000007 ?alkane_ind .
                    ?Reaction ontologies:reac4cat_000003 ?Catalyst}

	    INSERT {?Reaction :hasFollowUpReaction :catalysed_fischer_tropsch_process_ind}
	        WHERE {?Experiment rdf:type :CatalysisPerfomanceAssay .
	                ?Experiment :hasReaction ?Reaction .
	                ?alkane rdfs:label "alkane".
	                ?SubClassAlkane rdfs:subClassOf ?alkane .
	                ?alkane_ind rdf:type ?SubClassAlkane .
			?Dihydrogen rdf:type obo:CHEBI_18276 .
	                {?Reaction ontologies:reac4cat_000005 :Substance_CO .} UNION {?Reaction ontologies:reac4cat_000007 :Substance_CO .}
	                {?Reaction ontologies:reac4cat_000005 ?Dihydrogen .} UNION {?Reaction ontologies:reac4cat_000007 ?Dihydrogen .}
	                ?Reaction ontologies:reac4cat_000007 ?alkane_ind .
	                ?Reaction ontologies:reac4cat_000003 ?Catalyst .
	                FILTER NOT EXISTS {?Reaction :hasFirstReaction :catalysed_fischer_tropsch_process_ind}}

#FTS:
        INSERT {?Reaction :hasFirstReaction :fischer_tropsch_process_ind}
        WHERE {?Experiment rdf:type :CatalysisPerfomanceAssay .
            ?Experiment :hasReaction ?Reaction .
            ?alkane rdfs:label "alkane".
            ?SubClassAlkane rdfs:subClassOf ?alkane .
            ?alkane_ind rdf:type ?SubClassAlkane .
	        ?Dihydrogen rdf:type obo:CHEBI_18276 .
            ?Reaction ontologies:reac4cat_000005 :Substance_CO .
            ?Reaction ontologies:reac4cat_000005 ?Dihydrogen .
            ?Reaction ontologies:reac4cat_000007 ?alkane_ind .
            FILTER NOT EXISTS {?Reaction ontologies:reac4cat_000003 ?Catalyst}}

	INSERT {?Reaction :hasFollowUpReaction :fischer_tropsch_process_ind}
	WHERE {?Experiment rdf:type :CatalysisPerfomanceAssay .
            ?Experiment :hasReaction ?Reaction .
            ?alkane rdfs:label "alkane".
            ?SubClassAlkane rdfs:subClassOf ?alkane .
            ?alkane_ind rdf:type ?SubClassAlkane .
	        ?Dihydrogen rdf:type obo:CHEBI_18276 .
            {?Reaction ontologies:reac4cat_000005 :Substance_CO .} UNION {?Reaction ontologies:reac4cat_000007 :Substance_CO .}
            {?Reaction ontologies:reac4cat_000005 ?Dihydrogen .} UNION {?Reaction ontologies:reac4cat_000007 ?Dihydrogen .}
            ?Reaction ontologies:reac4cat_000007 ?alkane_ind .
            FILTER NOT EXISTS {?Reaction ontologies:reac4cat_000003 ?Catalyst}
            FILTER NOT EXISTS {?Reaction :hasFirstReaction :fischer_tropsch_process_ind}}

#Hyfo:
	    INSERT { ?Reaction :hasFirstReaction :hydroformylation_ind .}
        WHERE { ?Experiment rdf:type :CatalysisPerfomanceAssay .
                ?Experiment :hasReaction ?Reaction .
                ?alkene rdfs:label "alkene".
                ?SubClassAlkene rdfs:subClassOf ?alkene .
                ?alkene_ind rdf:type ?SubClassAlkene .
		        ?Dihydrogen rdf:type obo:CHEBI_18276 .
		        ?aldehyde rdfs:label "aldehyde" .
		        ?SubClassAldehyde rdfs:subClassOf ?aldehyde .
		        ?aldehyde_ind rdf:type ?SubClassAldehyde .
                ?Reaction ontologies:reac4cat_000005 :Substance_CO .
                ?Reaction ontologies:reac4cat_000005 ?Dihydrogen .
		        ?Reaction ontologies:reac4cat_000005 ?alkene_ind .
                ?Reaction ontologies:reac4cat_000007 ?aldehyde_ind .}

	    INSERT { ?Reaction :hasFollowUpReaction :hydroformylation_ind .}
        WHERE { ?Experiment rdf:type :CatalysisPerfomanceAssay .
                ?Experiment :hasReaction ?Reaction .
                ?alkene rdfs:label "alkene".
                ?SubClassAlkene rdfs:subClassOf ?alkene .
                ?alkene_ind rdf:type ?SubClassAlkene .
		        ?Dihydrogen rdf:type obo:CHEBI_18276 .
		        ?aldehyde rdfs:label "aldehyde" .
		        ?SubClassAldehyde rdfs:subClassOf ?aldehyde .
		        ?aldehyde_ind rdf:type ?SubClassAldehyde .
                ?Rhodium rdf:type obo:CHEBI_33359 .
                ?Cobalt rdf:type obo:CHEBI_155865 .
                ?Dihydrogen rdf:type obo:CHEBI_18276 .
                {?Reaction ontologies:reac4cat_000005 :Substance_CO .} UNION {?Reaction ontologies:reac4cat_000007 :Substance_CO .}
                {?Reaction ontologies:reac4cat_000005 ?Dihydrogen .} UNION {?Reaction ontologies:reac4cat_000007 ?Dihydrogen .}
		        {?Reaction ontologies:reac4cat_000005 ?alkene_ind .} UNION {?Reaction ontologies:reac4cat_000007 ?alkene_ind .}
                ?Reaction ontologies:reac4cat_000007 ?aldehyde_ind .
		        {?Reaction ontologies:reac4cat_000003 ?Cobalt .} UNION {?Reaction ontologies:reac4cat_000003 ?Rhodium .}
		        FILTER NOT EXISTS {?Reaction :hasFirstReaction :hydroformylation_ind}}

#Cativa process:

	    INSERT { ?Reaction :hasFirstReaction :cativa_process_ind .}
        WHERE { ?Experiment rdf:type :CatalysisPerfomanceAssay .
                ?Experiment :hasReaction ?Reaction .
                ?Aceticacid rdf:type obo:CHEBI_15366 .
		        ?Iridium rdf:type obo:CHEBI_37228 .
                ?Reaction ontologies:reac4cat_000005 :Substance_CO .
                ?Reaction ontologies:reac4cat_000005 :Substance_CH3OH .
                ?Reaction ontologies:reac4cat_000007 ?Aceticacid .
		        ?Reaction ontologies:reac4cat_000003 ?Iridium .}

	    INSERT { ?Reaction :hasFollowUpReaction :cativa_process_ind .}
        WHERE { ?Experiment rdf:type :CatalysisPerfomanceAssay .
                ?Experiment :hasReaction ?Reaction .
                ?Aceticacid rdf:type obo:CHEBI_15366 .
		        ?Iridium rdf:type obo:CHEBI_37228 .
                {?Reaction ontologies:reac4cat_000005 :Substance_CO .} UNION {?Reaction ontologies:reac4cat_000007 :Substance_CO .}
                {?Reaction ontologies:reac4cat_000005 :Substance_CH3OH .} UNION {?Reaction ontologies:reac4cat_000007 :Substance_CH3OH .}
                ?Reaction ontologies:reac4cat_000007 ?Aceticacid .
		        ?Reaction ontologies:reac4cat_000003 ?Iridium .
		        FILTER NOT EXISTS {?Reaction :hasFirstReaction :cativa_process_ind}}

#Monsanto process:

	    INSERT { ?Reaction :hasFirstReaction :monsanto_process_ind .}
        WHERE { ?Experiment rdf:type :CatalysisPerfomanceAssay .
                ?Experiment :hasReaction ?Reaction .
                ?Aceticacid rdf:type obo:CHEBI_15366 .
		        ?Rhodium rdf:type obo:CHEBI_33359 .
                ?Reaction ontologies:reac4cat_000005 :Substance_CO .
                ?Reaction ontologies:reac4cat_000005 :Substance_CH3OH .
                ?Reaction ontologies:reac4cat_000007 ?Aceticacid .
		        ?Reaction ontologies:reac4cat_000003 ?Rhodium .}

	    INSERT { ?Reaction :hasFollowUpReaction :monsanto_process_ind .}
        WHERE { ?Experiment rdf:type :CatalysisPerfomanceAssay .
                ?Experiment :hasReaction ?Reaction .
                ?Aceticacid rdf:type obo:CHEBI_15366 .
		        ?Rhodium rdf:type obo:CHEBI_33359 .
                {?Reaction ontologies:reac4cat_000005 :Substance_CO .} UNION {?Reaction ontologies:reac4cat_000007 :Substance_CO .}
                {?Reaction ontologies:reac4cat_000005 :Substance_CH3OH .} UNION {?Reaction ontologies:reac4cat_000007 :Substance_CH3OH .}
                ?Reaction ontologies:reac4cat_000007 ?Aceticacid .
		        ?Reaction ontologies:reac4cat_000003 ?Rhodium .
		        FILTER NOT EXISTS {?Reaction :hasFirstReaction :monsanto_process_ind}}

#catalysed Haber-Bosch:

    	    INSERT { ?Reaction :hasFirstReaction :catalysed_haber_bosch_reaction_ind .}
            WHERE { ?Experiment rdf:type :CatalysisPerfomanceAssay .
                    ?Experiment :hasReaction ?Reaction .
                    ?Dinitrogen rdf:type obo:CHEBI_17997 .
    		        ?Dihydrogen rdf:type obo:CHEBI_18276 .
    		        ?Ammonia rdf:type obo:CHEBI_16134 .
                    ?Reaction ontologies:reac4cat_000005 obo:CHEBI_17997 .
                    ?Reaction ontologies:reac4cat_000005 ?Dihydrogen .
                    ?Reaction ontologies:reac4cat_000007 ?Ammonia .
                    ?Reaction ontologies:reac4cat_000003 ?Catalyst}

    	    INSERT {?Reaction :hasFollowUpReaction :catalysed_haber_bosch_reaction_ind}
            WHERE {?Experiment rdf:type :CatalysisPerfomanceAssay .
                    ?Experiment :hasReaction ?Reaction .
                    ?Dinitrogen rdf:type obo:CHEBI_17997 .
    		        ?Dihydrogen rdf:type obo:CHEBI_18276 .
    		        ?Ammonia rdf:type obo:CHEBI_16134 .
                    {?Reaction ontologies:reac4cat_000005 ?Dinitrogen .} UNION {?Reaction ontologies:reac4cat_000007 ?Dinitrogen .}
                    {?Reaction ontologies:reac4cat_000005 ?Dihydrogen .} UNION {?Reaction ontologies:reac4cat_000007 ?Dihydrogen .}
                    ?Reaction ontologies:reac4cat_000007 ?Ammonia .
                    ?Reaction ontologies:reac4cat_000003 ?Catalyst .
                    FILTER NOT EXISTS {?Reaction :hasFirstReaction :catalysed_haber_bosch_reaction_ind}}

#Haber-Bosch:

            INSERT {?Reaction :hasFirstReaction :haber_bosch_reaction_ind}
            WHERE {?Experiment rdf:type :CatalysisPerfomanceAssay .
                ?Experiment :hasReaction ?Reaction .
                ?Dinitrogen rdf:type obo:CHEBI_17997 .
    	        ?Dihydrogen rdf:type obo:CHEBI_18276 .
    	         ?Ammonia rdf:type obo:CHEBI_16134 .
                ?Reaction ontologies:reac4cat_000005 ?Dinitrogen .
                ?Reaction ontologies:reac4cat_000005 ?Dihydrogen .
                ?Reaction ontologies:reac4cat_000007 ?Ammonia .
                FILTER NOT EXISTS {?Reaction ontologies:reac4cat_000003 ?Catalyst}}

            INSERT {?Reaction :hasFollowUpReaction :haber_bosch_reaction_ind}
            WHERE {?Experiment rdf:type :CatalysisPerfomanceAssay .
                ?Experiment :hasReaction ?Reaction .
                ?Dinitrogen rdf:type obo:CHEBI_17997 .
    	        ?Dihydrogen rdf:type obo:CHEBI_18276 .
    	        ?Ammonia rdf:type obo:CHEBI_16134 .
                {?Reaction ontologies:reac4cat_000005 ?Dinitrogen .} UNION {?Reaction ontologies:reac4cat_000007 ?Dinitrogen .}
                {?Reaction ontologies:reac4cat_000005 ?Dihydrogen .} UNION {?Reaction ontologies:reac4cat_000007 ?Dihydrogen .}
                ?Reaction ontologies:reac4cat_000007 ?Ammonia .
                FILTER NOT EXISTS {?Reaction ontologies:reac4cat_000003 ?Catalyst}
                FILTER NOT EXISTS {?Reaction :hasFirstReaction :haber_bosch_reaction_ind}}

#catalysed Methanation:

    	    INSERT { ?Reaction :hasFirstReaction :catalysed_methanation_reaction_ind .}
            WHERE { ?Experiment rdf:type :CatalysisPerfomanceAssay .
                    ?Experiment :hasReaction ?Reaction .
    		        ?Dihydrogen rdf:type obo:CHEBI_18276 .
    		        ?methane rdf:type obo:CHEBI_16183 .
    		        ?water rdf:type obo:CHEBI_15377 .
    		        ?carbondioxide rdf:type obo:CHEBI_16526 .
                    ?Reaction ontologies:reac4cat_000005 ?carbondioxide .
                    ?Reaction ontologies:reac4cat_000005 ?Dihydrogen .
                    ?Reaction ontologies:reac4cat_000007 ?methane .
                    ?Reaction ontologies:reac4cat_000007 ?water .
                    ?Reaction ontologies:reac4cat_000003 ?Catalyst}

    	    INSERT {?Reaction :hasFollowUpReaction :catalysed_methanation_reaction_ind}
            WHERE {?Experiment rdf:type :CatalysisPerfomanceAssay .
                    ?Experiment :hasReaction ?Reaction .
    		        ?Dihydrogen rdf:type obo:CHEBI_18276 .
    		        ?methane rdf:type obo:CHEBI_16183 .
    		        ?water rdf:type obo:CHEBI_15377 .
    		        ?carbondioxide rdf:type obo:CHEBI_16526 .
                    {?Reaction ontologies:reac4cat_000005 ?carbondioxide .} UNION {?Reaction ontologies:reac4cat_000007 ?carbondioxide .}
                    {?Reaction ontologies:reac4cat_000005 ?Dihydrogen .} UNION {?Reaction ontologies:reac4cat_000007 ?Dihydrogen .}
                    ?Reaction ontologies:reac4cat_000007 ?methane .
                    ?Reaction ontologies:reac4cat_000007 ?water .
                    ?Reaction ontologies:reac4cat_000003 ?Catalyst .
                    FILTER NOT EXISTS {?Reaction :hasFirstReaction :catalysed_methanation_reaction_ind}}

#Methanation:

                INSERT {?Reaction :hasFirstReaction :methanation_reaction_ind}
                WHERE {?Experiment rdf:type :CatalysisPerfomanceAssay .
                       ?Experiment :hasReaction ?Reaction .
    	               ?Dihydrogen rdf:type obo:CHEBI_18276 .
    	               ?methane rdf:type obo:CHEBI_16183 .
    	               ?water rdf:type obo:CHEBI_15377 .
    	               ?carbondioxide rdf:type obo:CHEBI_16526 .
                       ?Reaction ontologies:reac4cat_000005 ?carbondioxide .
                       ?Reaction ontologies:reac4cat_000005 ?Dihydrogen .
                       ?Reaction ontologies:reac4cat_000007 ?methane .
                       ?Reaction ontologies:reac4cat_000007 ?water .
                       FILTER NOT EXISTS {?Reaction ontologies:reac4cat_000003 ?Catalyst}}

                    INSERT {?Reaction :hasFollowUpReaction :methanation_reaction_ind}
                    WHERE  {?Experiment rdf:type :CatalysisPerfomanceAssay .
                            ?Experiment :hasReaction ?Reaction .
                            ?Dihydrogen rdf:type obo:CHEBI_18276 .
                            ?methane rdf:type obo:CHEBI_16183 .
                            ?water rdf:type obo:CHEBI_15377 .
                            ?carbondioxide rdf:type obo:CHEBI_16526 .
                            {?Reaction ontologies:reac4cat_000005 ?carbondioxide .} UNION {?Reaction ontologies:reac4cat_000007 ?carbondioxide .}
                            {?Reaction ontologies:reac4cat_000005 ?Dihydrogen .} UNION {?Reaction ontologies:reac4cat_000007 ?Dihydrogen .}
                            ?Reaction ontologies:reac4cat_000007 ?methane .
                            ?Reaction ontologies:reac4cat_000007 ?water .
                            FILTER NOT EXISTS {?Reaction ontologies:reac4cat_000003 ?Catalyst}
                            FILTER NOT EXISTS {?Reaction :hasFirstReaction :methanation_reaction_ind}}
```
