# =============================================================================
# REPLICATION FILE: data/raw/code/Makefile
# VERSION: zsh
# AUTHOR: Victor Alfonso Ortega Le Hénanff
# EMAIL: vincictor33@gmail.com
# DATE: 2026-01-14
# =============================================================================

# Combined Makefile for downloading all raw data sources
# INPC deflators, ENIGH surveys, and CONEVAL poverty data

# Configuration
TOKEN := d024134c-be50-03ee-bdf6-1cda05612673
INDICATOR_ID := 910392
API_URL := https://en.www.inegi.org.mx/app/api/indicadores/desarrolladores/jsonxml/INDICATOR/$(INDICATOR_ID)/en/0700/false/BIE/2.0/$(TOKEN)?type=json

YEARS := 2024 2022 2020 2018 2016
YEARS_CONEVAL := 2022 2020 2018 2016

# All targets
all: inpc enigh poverty
	rm -rf ../temp

# INPC deflator data
inpc: ../output/inpc.json

# ENIGH survey data
enigh: $(addprefix ../output/, \
     concentradohogar2024.dta ingresos2024.dta poblacion2024.dta trabajos2024.dta viviendas2024.dta \
     concentradohogar2022.dta ingresos2022.dta poblacion2022.dta trabajos2022.dta viviendas2022.dta \
     concentradohogar2020.dta ingresos2020.dta poblacion2020.dta trabajos2020.dta viviendas2020.dta \
     concentradohogar2018.dta ingresos2018.dta poblacion2018.dta trabajos2018.dta viviendas2018.dta \
     concentradohogar2016.dta ingresos2016.dta poblacion2016.dta trabajos2016.dta viviendas2016.dta)

# CONEVAL poverty data
poverty: $(addprefix ../output/, \
     pobreza2022.dta pobreza2020.dta pobreza2018.dta pobreza2016.dta)

# Directory creation
../temp:
	mkdir -p ../temp

../input:
	mkdir -p ../input

../output:
	mkdir -p ../output

# =============================================================================
# INPC DEFLATOR RULES
# =============================================================================

../output/inpc.json: | ../output
	curl -L -o ../output/inpc.json $(API_URL)

# =============================================================================
# ENIGH SURVEY RULES
# =============================================================================

# Define rule template for concentradohogar
define concentradohogar_rules
../temp/enigh$(1)_dta.zip: | ../temp
	wget https://www.inegi.org.mx/contenidos/programas/enigh/nc/$(1)/microdatos/enigh$(1)_ns_concentradohogar_dta.zip -O $$@ || { echo "Failed to download $(1) concentradohogar"; exit 1; }

../output/concentradohogar$(1).dta: ../temp/enigh$(1)_dta.zip | ../output
	unzip -o -j $$< concentradohogar.dta -d ../output
	mv ../output/concentradohogar.dta $$@
endef

# Define rule template for ingresos
define ingresos_rules
../temp/enigh$(1)_i_dta.zip: | ../temp
	wget https://www.inegi.org.mx/contenidos/programas/enigh/nc/$(1)/microdatos/enigh$(1)_ns_ingresos_dta.zip -O $$@ || { echo "Failed to download $(1) ingresos"; exit 1; }

../output/ingresos$(1).dta: ../temp/enigh$(1)_i_dta.zip | ../output
	unzip -o -j $$< ingresos.dta -d ../output
	mv ../output/ingresos.dta $$@
endef

# Define rule template for poblacion
define poblacion_rules
../temp/enigh$(1)_p_dta.zip: | ../temp
	wget https://www.inegi.org.mx/contenidos/programas/enigh/nc/$(1)/microdatos/enigh$(1)_ns_poblacion_dta.zip -O $$@ || { echo "Failed to download $(1) poblacion"; exit 1; }

../output/poblacion$(1).dta: ../temp/enigh$(1)_p_dta.zip | ../output
	unzip -o -j $$< poblacion.dta -d ../output
	mv ../output/poblacion.dta $$@
endef

# Define rule template for trabajos
define trabajos_rules
../temp/enigh$(1)_t_dta.zip: | ../temp
	wget https://www.inegi.org.mx/contenidos/programas/enigh/nc/$(1)/microdatos/enigh$(1)_ns_trabajos_dta.zip -O $$@ || { echo "Failed to download $(1) trabajos"; exit 1; }

../output/trabajos$(1).dta: ../temp/enigh$(1)_t_dta.zip | ../output
	unzip -o -j $$< trabajos.dta -d ../output
	mv ../output/trabajos.dta $$@
endef

# Define rule template for viviendas
define viviendas_rules
../temp/enigh$(1)_v_dta.zip: | ../temp
	wget https://www.inegi.org.mx/contenidos/programas/enigh/nc/$(1)/microdatos/enigh$(1)_ns_viviendas_dta.zip -O $$@ || { echo "Failed to download $(1) viviendas"; exit 1; }

../output/viviendas$(1).dta: ../temp/enigh$(1)_v_dta.zip | ../output
	unzip -o -j $$< viviendas.dta -d ../output
	mv ../output/viviendas.dta $$@
endef

# =============================================================================
# CONEVAL POVERTY RULES
# =============================================================================

# Define rule template for poverty data
define poverty_rules
../temp/STATA_MMP_$(1).zip: | ../temp
	@if [ "$(1)" = "2022" ]; then \
		wget -c -O $$@ https://www.coneval.org.mx/Medicion/MP/Documents/MMP_2022/Programas_calculo/STATA_MMP_2022.zip; \
	elif [ "$(1)" = "2020" ]; then \
		wget -c -O $$@ https://www.coneval.org.mx/Medicion/MP/Documents/Programas_calculo_pobreza_MMP_20/STATA_MMP_2020.zip; \
	elif [ "$(1)" = "2018" ]; then \
		wget -c -O $$@ https://www.coneval.org.mx/Medicion/MP/Documents/Programas_calculo_pobreza_MMP_18/STATA_MMP_2018.zip; \
	elif [ "$(1)" = "2016" ]; then \
		wget -c -O $$@ https://www.coneval.org.mx/Medicion/MP/Documents/Programas_calculo_pobreza_MMP_16/STATA_MMP_2016.zip; \
	fi

../output/pobreza$(1).dta: ../temp/STATA_MMP_$(1).zip | ../output
	unzip -j -o ../temp/STATA_MMP_$(1).zip "*/pobreza_*.dta" -d ../output || unzip -j -o ../temp/STATA_MMP_$(1).zip "Base final/pobreza_*.dta" -d ../output
	mv ../output/pobreza_*.dta $$@
endef

# =============================================================================
# GENERATE RULES FOR ALL YEARS
# =============================================================================

$(foreach year,$(YEARS),$(eval $(call concentradohogar_rules,$(year))))
$(foreach year,$(YEARS),$(eval $(call ingresos_rules,$(year))))
$(foreach year,$(YEARS),$(eval $(call poblacion_rules,$(year))))
$(foreach year,$(YEARS),$(eval $(call trabajos_rules,$(year))))
$(foreach year,$(YEARS),$(eval $(call viviendas_rules,$(year))))
$(foreach year,$(YEARS_CONEVAL),$(eval $(call poverty_rules,$(year))))

# =============================================================================
# PHONY TARGETS
# =============================================================================

.PHONY: all inpc enigh poverty clean

clean:
	rm -rf ../temp

# Help target
help:
	@echo "Available targets:"
	@echo "  all     - Download all data (INPC, ENIGH, and CONEVAL poverty)"
	@echo "  inpc    - Download only INPC deflator data"
	@echo "  enigh   - Download only ENIGH survey data"
	@echo "  poverty - Download only CONEVAL poverty data"
	@echo "  clean   - Remove all downloaded data and temp files"