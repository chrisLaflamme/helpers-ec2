require 'aws-sdk'
require 'colorize'

# list of AWS regions
$regions = [
  'us-east-1',
  'us-east-2',
  'us-west-1',
  'us-west-2',
  'ca-central-1',
  'eu-west-1',
  'eu-central-1',
  'eu-west-2',
  'ap-northeast-1',
  'ap-northeast-2',
  'ap-southeast-1',
  'ap-southeast-2',
  'ap-south-1',
  'sa-east-1'
]
#
# Method Definitions
############################################################################################
def show_all_instances
  # intialize empty hash to push to
  regions_with_instances = {}
  $regions.each do |region|
    # new client for every region
    ec2 = Aws::EC2::Client.new(
      region: region
    )
    resp = ec2.describe_instances(max_results: 500)
    # check to see if there are any instances in the region at all
    if resp.reservations[0].nil?
      puts "#{region}: No instances".upcase.colorize(:red)
      regions_with_instances[region.to_s] = nil
    else
      puts "#{region}:".upcase.colorize(:green)
      num_res = resp.reservations.count
      num_res.times do |i|
        # Loop through all the tags and print the "Name"
        num_tags = resp.reservations[i].instances[0].tags.count
        num_tags.times do |t|
          if resp.reservations[i].instances[0].tags[t].key == "Name"
            puts "\t\tBOX NAME:\t" + resp.reservations[i].instances[0].tags[t].value
          end
        end
        puts "\t\tINSTANCE TYPE:\t" + resp.reservations[i].instances[0].instance_type
        puts "\t\tINSTANCE ID:\t" + resp.reservations[i].instances[0].instance_id
        puts "\t\tSTATE:\t\t" + resp.reservations[0].instances[0].state.name.upcase
        puts "\t\t=================================".colorize(:blue)
      end
      regions_with_instances[region.to_s] = resp.reservations[0].instances[0].tags[0].value
    end
  end
end
#
# Rake Tasks
############################################################################################
desc 'List all Ec2 instances for all regions'
task :list_all_instances do
  show_all_instances()
end
