require 'aws-sdk-ec2'
require 'colorize'
#
# Method Definitions
############################################################################################
# list of AWS regions
def list_regions
  ec2 = Aws::EC2::Client.new(
    region: 'eu-west-1'
  )
  regions = []
  # list all AWS regions
  regions_resp = ec2.describe_regions
  regions_resp.regions.each do |region|
    regions.push(region.region_name)
  end
  regions.sort!
end

def show_all_instances
  regions_with_instances = {} # intialize empty hash to push to
  regions = list_regions # call list_regions def to build regions[]
  regions.each do |region|
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
  show_all_instances
end
