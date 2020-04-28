# demo
//1. 读取车辆运动轨迹
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
2. 对于每一个运动轨迹点【for(auto p: driveDatas)】
2.1 读取入口GPS文件（仅初始化执行一次），根据存储的入口GPS数据，计算均值（storage和origin之间的距离必须小于指定阈值）已更新GPS的mark位置，并根据origin和mark判断当前位置是否远离当前入口。此外，根据车辆当前GPS信号与入口mark之间的距离，判断是否开始计算变换矩阵。
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
2.2 读取地图数据（仅初始化执行一次）
vector<Joints_type> vJoints;
vector<Way_points_type> vWay_points;
vector<Slots_type> vSlots;
vector<Slots_type> vSlots_RT;
vector<Landmarks_type> vLandmarks;
2.3 计算SCU->ENU的坐标变换矩阵MtxT
2.3.1 利用GPS_origin初始化ENU坐标系，当车辆当前GPS信号与入口mark之间的距离（40-150m），且车辆具有一定的车速，以及GPS信号的准确率<2m时，计算车辆相对于origin在东北天坐标系下ENU的位置Gpsloclst，并存储对应的里程计位置Sculoclst（仅存储100个数据）
2.3.2 当车辆当前GPS信号与入口mark之间的距离小于40m时，根据Gpsloclst和Sculoclst来计算SCU->ENU的坐标变换矩阵MtxT；
2.3.3 根据上面计算的坐标变换矩阵得到SCU在ENU坐标系下的位置location；
2.4 在新地图中添加路径点waypoints
将经过SCU->ENU坐标变换的车辆轨迹driveData存储在buffer_maxSize（仅存储300个数据）和winRaw_maxSize列表中；并将轨迹点location保存到当前地图map_traj->vWay_poin
2.5 根据winRaw_maxSize判断路标特征的置信度
2.5.1 当winRaw_maxSize中vWay_points的起始位置ds>3m时，计算winRaw_maxSize中的数据的平均速度Spd，转向角Eps，曲率Curvature，俯仰角pitch，GPS精度GpsAccu，偏航角yaw..。
2.5.5 在当前winRaw_maxSize中，根据对应的指标，判断路标类型(减速带，上下坡，)和对应的置信度，并在buffer_maxSize中确定路标的位置location和index

2.6 在新地图中添加车位Slot
在buffer_maxSize中查找距离当前轨迹点处所检测到的slot距离最小的waypoint_ind，并创建slot存储到当前地图map_traj->vSlots，如果已经添加到列表中，则无需再增加
2.7 在新地图中添加车位SlotRT
