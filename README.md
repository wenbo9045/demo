# demo
//1,读取车辆运动轨迹
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
2.对于每一个运动轨迹点for(auto p: driveDatas)
2.1 读取入口GPS文件，判断是否越过当前入口（GPS_origin，GPS_entrance）
Gps_origin_out = origin；Gps_mark_out = mean(Gps_entrance_mark)
typedef struct
{
	double GPS_Longitude;
	double GPS_Latitude;
	double GPS_Accuracy;
    int Gps_day_hour;
} GPS_DataType;

class GPS_save_class{
public:
	GPS_DataType GPS_origin;
	GPS_DataType GPS_mark;
	int write_flag;
	int Gps_storage_count;
	int Gps_count;
	int time_day;
	unsigned int entrance_count;
	list<GPS_DataType> Gps_storage;
} ;
2.2读取地图数据
vector<Joints_type> vJoints;
vector<Way_points_type> vWay_points;
vector<Slots_type> vSlots;
vector<Slots_type> vSlots_RT;
vector<Landmarks_type> vLandmarks;
