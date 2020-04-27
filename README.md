# demo
//1,读取车辆运动路径
typedef struct {
    float SCU_Locat_x;//车辆位置
    float SCU_Locat_y;//车辆位置
    float SCU_Locat_z;//车辆位置
    float SCU_Locat_theta;//车辆航向
    float SCU_Locat_S;//里程
    float SCU_Locat_CurSpd;//车速
    float SCU_Locat_theta_roll;//转角 
    float SCU_Locat_theta_pitch;//辅仰角
    float SCU_ParkingPt_NearRear_X;// 车位B点
    float SCU_ParkingPt_NearRear_Y;//车位B点
    float SCU_ParkingPt_NearFront_X;//车位E点
    float SCU_ParkingPt_NearFront_Y;//车位E点
    int AVM_Slot_Type;//车位形状
    int CDU_Slot_Type;//车位属性
    int VCU_CurrentGearLev;//当前挡位
    float EPS_SAS_SteeringAngle;//方向转角
    int BCM_DriverDroorLockSt;//  锁车状态
    int power_Model;// 下电状态
    float CDU_SCU_GPS_LONG_itude;// GPS 经度
    float CDU_SCU_GPS_Latitude;//纬度
    float CDU_SCU_GPS_ALtitude;// GPS 高度
    float CDU_SCU_GPS_Accuracy;//精度
    char *occurs_time;//当前时间
	  unsigned short parking_save_state;//pkmap保存状态
} DrivingDataType;
