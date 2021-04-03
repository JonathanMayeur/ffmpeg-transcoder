FROM jrottenberg/ffmpeg:3.4-alpine312

WORKDIR /local/bin/ffmpeg

ENV input=''
ENV rtbufsize='512M'
ENV size1='1920x1080'
ENV size2='1280x720'
ENV size3='854x480'
ENV size4='426x240'
ENV preset1='faster'
ENV preset2='faster'
ENV preset3='faster'
ENV preset4='faster'
ENV level1='4.0'
ENV level2='4.0'
ENV level3='4.0'
ENV level4='4.0'
ENV gv='100'
ENV bv1='2000k'
ENV bv2='1200k'
ENV bv3='800k'
ENV bv4='400k'
ENV ba1='160k'
ENV ba2='160k'
ENV ba3='128k'
ENV ba4='96k'
ENV output1=
ENV output2=
ENV output3=
ENV output4=

ENTRYPOINT ffmpeg -rtbufsize ${rtbufsize} ${input} -filter_complex '[0:v]split=4[v1][v2][v3][v4];[0:a]asplit=4[a1][a2][a3][a4]' \
        -map '[v1]' -map '[a1]' -vcodec libx264 -s ${size1} -preset:v ${preset1} -level:v ${level1} -b:v ${bv1} -g:v ${gv} -b:a ${ba1} -acodec libfdk_aac -f flv ${output1} \
        -map '[v2]' -map '[a2]' -vcodec libx264 -s ${size2} -preset:v ${preset2} -level:v ${level2} -b:v ${bv2} -g:v ${gv} -b:a ${ba2} -acodec libfdk_aac -f flv ${output2} \
        -map '[v3]' -map '[a3]' -vcodec libx264 -s ${size3} -preset:v ${preset3} -level:v ${level3} -b:v ${bv3} -g:v ${gv} -b:a ${ba3} -acodec libfdk_aac -f flv ${output3} \
        -map '[v4]' -map '[a4]' -vcodec libx264 -s ${size4} -preset:v ${preset4} -level:v ${level4} -b:v ${bv4} -g:v ${gv} -b:a ${ba4} -acodec libfdk_aac -f flv ${output4}