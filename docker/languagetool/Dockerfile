FROM ubuntu as ftbuild

RUN apt update && apt upgrade -y && apt install -y \
        build-essential \
        wget \
        git \
        make \
        unzip


RUN git clone https://github.com/facebookresearch/fastText.git /tmp/fastText && \
  rm -rf /tmp/fastText/.git* && \
  mv /tmp/fastText/* / && \
  cd / && \
  make

RUN wget https://dl.fbaipublicfiles.com/fasttext/supervised-models/lid.176.bin

RUN wget https://languagetool.org/download/ngram-lang-detect/model_ml50_new.zip

FROM erikvl87/languagetool

COPY --chown=languagetool --from=ftbuild /fasttext .
COPY --chown=languagetool --from=ftbuild /model_ml50_new.zip .
COPY --chown=languagetool --from=ftbuild /lid.176.bin .

ENV Java_Xms=1g
ENV Java_Xmx=2g
ENV langtool_languageModel=/ngrams
ENV langtool_fasttextBinary=/LanguageTool/fasttext
ENV langtool_ngramLangIdentData=/LanguageTool/model_ml50_new.zip
ENV langtool_fasttextModel=/LanguageTool/lid.176.bin