# Simplified pipeline guide
This doc shows how to simplify pipeline creation using the new API.

原先带的三个pipeline，现在都用新的api创建了一次，降低心智负担的同时，提高效率。
- [sample_vin_ivps_joint_vo](../examples/sample_vin_ivps_joint_vo) -> [sample_vin_ivps_joint_vo_new](../examples/sample_vin_ivps_joint_vo_new)
- [sample_vin_ivps_joint_venc_rtsp](../examples/sample_vin_ivps_joint_venc_rtsp) ->  [sample_vin_ivps_joint_venc_rtsp_new](../examples/sample_vin_ivps_joint_venc_rtsp_new)
- [sample_vin_ivps_joint_venc_rtsp_vo](../examples/sample_vin_ivps_joint_venc_rtsp_vo) -> [sample_vin_ivps_joint_venc_rtsp_vo_new](../examples/sample_vin_ivps_joint_venc_rtsp_vo_new)


Example: Create a pipeline that reads from camera and outputs an RTSP stream
```c++
pipeline_t pipe_rtsp;
{
    pipeline_ivps_config_t &config = pipe_rtsp.m_ivps_attr;
    config.n_ivps_grp = 0;
    config.n_ivps_fps = 25;
    config.n_ivps_width = 1920;
    config.n_ivps_height = 1080;
    config.n_osd_rgn = 4; // Number of OSD regions (max 5). Each region can display up to 32 objects. When using a custom RGBA canvas, one object may occupy a slot in a region, so here we only create one.
}
pipe_rtsp.enable = 1;
pipe_rtsp.pipeid = 0x90015;
pipe_rtsp.m_input_type = pi_vin;
pipe_rtsp.m_output_type = po_rtsp_h264; //可以创建265，降低带宽压力
pipe_rtsp.n_loog_exit = 0; // 可以用来控制线程退出（如果有的话）
pipe_rtsp.n_vin_pipe = 0;
pipe_rtsp.n_vin_chn = 0;
sprintf(pipe_rtsp.m_venc_attr.end_point, "axstream0"); // 重复的会创建失败
pipe_rtsp.m_venc_attr.n_venc_chn = 0;                  // 重复的会创建失败
create_pipeline(&pipe_rtsp);
```

Example: Create a pipeline that reads from camera, resizes with letterbox padding to the desired resolution, and runs AI inference
```c++
void ai_inference_func(pipeline_buffer_t *buff)
{
    static sample_run_joint_results mResults;
    AX_NPU_CV_Image tSrcFrame = {0};

    tSrcFrame.eDtype = (AX_NPU_CV_FrameDataType)buff->d_type;
    tSrcFrame.nWidth = buff->n_width;
    tSrcFrame.nHeight = buff->n_height;
    tSrcFrame.pVir = (unsigned char *)buff->p_vir;
    tSrcFrame.pPhy = buff->p_phy;
    tSrcFrame.tStride.nW = buff->n_stride;
    tSrcFrame.nSize = buff->n_size;

    sample_run_joint_inference_single_func(gModels, &tSrcFrame, &mResults);
}
...

pipeline_t pipe_ai;
{
    pipeline_ivps_config_t &config = pipe_ai.m_ivps_attr;
    config.n_ivps_grp = 1; // Duplicate groups will fail to create
    config.n_ivps_fps = 60;
    config.n_ivps_width = 640;
    config.n_ivps_height = 640;
    config.b_letterbox = 1;   // Enable letterbox padding and scaling
    config.n_fifo_count = 1;  // If you want to receive frame callbacks, set this to 1~4
}
pipe_ai.enable = 1;
pipe_ai.pipeid = 0x90016;
pipe_ai.m_input_type = pi_vin;
switch (gModels.SAMPLE_ALGO_FORMAT)
{
case AX_FORMAT_RGB888:
    pipe_ai.m_output_type = po_buff_rgb;
    break;
case AX_FORMAT_BGR888:
    pipe_ai.m_output_type = po_buff_bgr;
    break;
case AX_YUV420_SEMIPLANAR:
default:
    pipe_ai.m_output_type = po_buff_nv12;
    break;
}
pipe_ai.n_loog_exit = 0;
pipe_ai.n_vin_pipe = 0;
pipe_ai.n_vin_chn = 0;
    pipe_ai.output_func = ai_inference_func; // Callback function to handle image output
create_pipeline(&pipe_ai);
```

Example: Create a pipeline that reads from camera and outputs to the AiXinPai screen
```c++
 pipeline_t pipe_vo;
{
    pipeline_ivps_config_t &config = pipe_vo.m_ivps_attr;
    config.n_ivps_grp = 0;    // 重复的会创建失败
    config.n_ivps_fps = 60;   // Screen output is limited to 60 fps
    config.n_ivps_rotate = 1; // 旋转
    config.n_ivps_width = 854;
    config.n_ivps_height = 480;
    config.n_osd_rgn = 4; // Number of OSD regions, each region can display up to 32 objects
}
pipe_vo.enable = 1;
pipe_vo.pipeid = 0x90015;
pipe_vo.m_input_type = pi_vin;
pipe_vo.m_output_type = po_vo_sipeed_maix3_screen;
    pipe_vo.n_loog_exit = 0; // Can be used to control thread exit (if any)
pipe_vo.n_vin_pipe = 0;
pipe_vo.n_vin_chn = 0;
create_pipeline(&pipe_vo);
```

Everything becomes clearer and simpler.
## For pipeline_t structure and parameters, see the header file documentation:
[header reference](../examples/common/common_pipeline/common_pipeline.h)
