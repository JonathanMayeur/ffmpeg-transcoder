# Docker FFmpeg transcoder
An FFmpeg video transcoder to create an ABR stream.

The transcoder uses an FFmpeg dockerfile built from source. Built on Alpine Linux. The docker file can be found [alfg/docker-ffmpeg](https://github.com/alfg/docker-ffmpeg).

# Usage
The ffmpeg command uses the `-filter_complex` to split the input source to 3 different sources. The `-map` option instructs ffmpeg what streams from the inputs created via the splitt filter, should be included in the outputs.

The source input is set via an environment variable `${input}`. We can set the real-time buffer with the `${rtbufsize}`. The Dockerfile comes with the input test input value to test the transcoder. This can be replaced with the input you want to transcode.

The `${size}` variable allows to resize the input source. With `${preset}` we can establish the encoding speed. With a slower spectrum you get more quality at the sacrifice of time and CPU usage and quality. `${level}` specifies the encoding level. With `${bv}` and `${ba}` we specify the video and audio encoding bit rate.

The `${output}` fields allow us to set the rtmp outputs.

## Docker file
```Dockerfile
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
ENV bv1='1250k'
ENV bv2='900k'
ENV bv3='450k'
ENV ba1='160k'
ENV ba2='160k'
ENV ba3='128k'
ENV output1=
ENV output2=
ENV output3=

ENTRYPOINT ffmpeg -rtbufsize ${rtbufsize} ${input} -filter_complex '[0:v]split=3[v1][v2][v3];[0:a]asplit=3[a1][a2][a3]' \
        -map '[v1]' -map '[a1]' -vcodec libx264 -s ${size1} -preset:v ${preset1} -level:v ${level1} -b:v ${bv1} -b:a ${ba1} -acodec libfdk_aac -f flv ${output1} \
        -map '[v2]' -map '[a2]' -vcodec libx264 -s ${size2} -preset:v ${preset2} -level:v ${level2} -b:v ${bv2} -b:a ${ba2} -acodec libfdk_aac -f flv ${output2} \
        -map '[v3]' -map '[a3]' -vcodec libx264 -s ${size3} -preset:v ${preset3} -level:v ${level3} -b:v ${bv3} -b:a ${ba3} -acodec libfdk_aac -f flv ${output3}
```

# Build and run
After you changed the Dockerfile in your repo, you can build and run the container from source.

```bash
$ docker build -t ffmpeg_transcoder .
$ docker run --name my_ffmpeg_transcoder -it ffmpeg_transcoder
```

If needed you can run a docker image with the IP address of the host mapped to the container.

```bash
$ docker run -d --network host --name ffmpeg-transcoder-host IMAGE_ID
```

Because we set the parameters via environment variables, these can be overwritten when starting the docker container.

# License
[MIT](https://choosealicense.com/licenses/mit/)