.PHONY : clean, default, publish

REPO_NAME   = ${TRAVIS_REPO_SLUG#*/}
TEX_FILES   = ${REPO_NAME}
TMP_SUFFS   = pdf aux bbl blg log dvi ps eps out ent
LATEX       = pdflatex -interaction=nonstopmode -halt-on-error
SUFF        = pdf
RM_TMP      = $(foreach d, ${TEX_FILES}, rm -rf $(foreach suff, ${TMP_SUFFS}, ${d}.${suff})) ${REPO_NAME}Notes.bib
CHECK_RERUN = grep Rerun ${REPO_NAME}.log
FIGURES     = $(patsubst %.py,%.pdf,$(wildcard figures/*.py))
TECTONIC    = $(shell command -v tectonic >/dev/null && echo true || echo false )

default: ${REPO_NAME}.pdf

${REPO_NAME}.pdf: ${REPO_NAME}.tex bib.bib $(FIGURES)
	# Generate links to current git commit
	#python gen_links.py
	# Compile the paper with either tectonic or pdflatex
	if [ "${TECTONIC}" = "true" ]; then\
		tectonic ${REPO_NAME}.tex --print --keep-logs;\
	else\
		${LATEX} ${REPO_NAME}.tex;\
		( ${CHECK_RERUN} && ${LATEX} ${REPO_NAME}.tex ) || echo "Done.";\
		( ${CHECK_RERUN} && ${LATEX} ${REPO_NAME}.tex ) || echo "Done.";\
		( ${CHECK_RERUN} && ${LATEX} ${REPO_NAME}.tex ) || echo "Done.";\
	fi

clean:
	# Remove temporaries and output
	$(RM_TMP)
	rm -rf $(FIGURES)

figures/%.pdf: figures/%.py
	# Generate all figures
	cd $(<D);python $(<F)
