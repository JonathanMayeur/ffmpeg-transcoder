FROM alfg/ffmpeg:latest

WORKDIR /opt/ffmpeg/bin

ENV input='-f lavfi -i testsrc=duration=5000:size=1280x720:rate=30'
ENV rtbufsize='128M'
ENV size1='1280x720'
ENV size2='854x480'
ENV size3='426x240'
ENV preset1='faster'
ENV preset2='faster'
ENV preset3='faster'
ENV level1='4.0'
ENV level2='4.0'
ENV level3='4.0'
ENV bv1='1200k'
ENV bv2='800k'
ENV bv3='400k'
ENV ba1='160k'
ENV ba2='128k'
ENV ba3='96k'
ENV output1=
ENV output2=
ENV output3=

ENTRYPOINT ffmpeg -rtbufsize ${rtbufsize} ${input} -filter_complex '[0:v]split=3[v1][v2][v3];[0:a]asplit=3[a1][a2][a3]' \
        -map '[v1]' -map '[a1]' -vcodec libx264 -s ${size1} -preset:v ${preset1} -level:v ${level1} -b:v ${bv1} -b:a ${ba1} -acodec libfdk_aac -f flv ${output1} \
        -map '[v2]' -map '[a2]' -vcodec libx264 -s ${size2} -preset:v ${preset2} -level:v ${level2} -b:v ${bv2} -b:a ${ba2} -acodec libfdk_aac -f flv ${output2} \
        -map '[v3]' -map '[a3]' -vcodec libx264 -s ${size3} -preset:v ${preset3} -level:v ${level3} -b:v ${bv3} -b:a ${ba3} -acodec libfdk_aac -f flv ${output3}